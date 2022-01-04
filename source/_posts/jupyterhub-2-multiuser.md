---
title: 利用 jupyterhub 搭建多用户 python 开发环境
mathjax: true
author: Jinzhong Xu
date: 2021-12-21 17:03:46
tags:
- python
- jupyterhub
- jupyterlab
categories:
- technology
- python
---

JupyterHub 是为多个用户提供 JupyterLab 的集成服务系统，每个用户相互隔离。在 JupyterHub 版本低于 2.0.0 时，可采用[该方法](https://xujinzh.github.io/2021/02/20/jupyterhub)部署。本篇介绍对于最新版 JupyterHub 2.0.0 （截止到2021年12月21日）的部署方法。相关环境是 miniconda, wsl (ubuntu)，所有命令以 root 用户运行。

<!--more-->

# 安装 miniconda

```bash
# 切换到主目录，下载miniconda并安装
cd 
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
chmod +x Miniconda3-latest-Linux-x86_64.sh
./Miniconda3-latest-Linux-x86_64.sh
# 选择安装在 /usr/local/miniconda
echo 'export PATH=/usr/local/miniconda/bin:$PATH' >> /etc/profile
source /etc/profile
echo 'export PATH=/usr/local/miniconda/bin:$PATH' >> .bashrc
# 同时添加到普通用户，如 jinzhongxu
echo 'export PATH=/usr/local/miniconda/bin:$PATH' >> /home/jinzhongxu/.bashrc
# 更新环境变量
source .bashrc
# 添加用户访问权限
groupadd miniconda
chgrp -R miniconda /usr/local/miniconda
chmod 770 -R /usr/local/miniconda
usermod -a -G miniconda root
usermod -a -G miniconda jinzhongxu

# 如果添加新的用户(如 admin)使用 jupyterhub，后续只需要将该用户 admin 添加到组 miniconda 中即可，方法如下
# sudo usermod -a -G miniconda admin
# 删除用户方法如下，即把用户admin从组miniconda中删除。然后重启jupyterhub
# sudo gpasswd -d admin miniconda

# 安装常用包
pip install --upgrade pip 
pip install jupyterhub jupyterlab pandas matplotlib youtube_dl numba boost scipy numpy sympy scikit-learn opencv-contrib-python Pillow
pip install jupyterlab_code_formatter;jupyter server extension enable jupyterlab_code_formatter;pip install black isort

# 安装 configurable-http-proxy
apt install nodejs npm -y
npm install -g configurable-http-proxy

# 生成配置文件
jupyterhub --generate-config
mkdir -p /etc/jupyterhub
mv jupyterhub_config.py /etc/jupyterhub/config.py
# 使得普通用户jinzhongxu也能够在该目录创建文件
chown -R jinzhongxu /etc/jupyterhub/

# 设置 wsl 为base url, jinzhongxu 为管理员用户
echo "c.JupyterHub.base_url = '/wsl'" >> /etc/jupyterhub/config.py
echo "c.JupyterHub.hub_ip = '0.0.0.0'" >> /etc/jupyterhub/config.py
echo "c.Authenticator.delete_invalid_users = True" >> /etc/jupyterhub/config.py
echo "c.Authenticator.admin_users = set(['jinzhongxu'])" >> /etc/jupyterhub/config.py
echo "c.JupyterHub.admin_access = True" >> /etc/jupyterhub/config.py
echo "c.Spawner.cmd = ['/usr/local/miniconda/bin/jupyterhub-singleuser']" >> /etc/jupyterhub/config.py
echo "c.JupyterHub.authenticator_class = 'jupyterhub.auth.DummyAuthenticator'"  >> /etc/jupyterhub/config.py
```

注意：

1. 如果之前使用过 jupyter，需删除 /root/.jupyter 和 /home/jinzhongxu/.jupyter
2. 如果使用普通用户启动jupyterhub，需要该用户有使用configurable-http-proxy的权限


# 配置启动

因为 wsl 不能使用 systemctl，这里使用 service，创建文件 `/etc/init.d/jupyterhub`，添加如下内容

```bash
#! /bin/sh
### BEGIN INIT INFO
# Provides:          jupyterhub
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start jupyterhub
# Description:       This file should be used to construct scripts to be
#                    placed in /etc/init.d.
### END INIT INFO

# Author: JX
#
# Please remove the "Author" lines above and replace them
# with your own name if you copy and modify this script.

# Do NOT "set -e"

# PATH should only include /usr/* if it runs after the mountnfs.sh script
PATH=/sbin:/usr/local/bin:/usr/local/miniconda/bin:/usr/sbin:/bin:/usr/bin
DESC="Multi-user server for Jupyter notebooks"
NAME=jupyterhub
DAEMON=/usr/local/miniconda/bin/jupyterhub
DAEMON_ARGS="--config=/etc/jupyterhub/config.py"
WORKING_DIR="/etc/jupyterhub"
PIDFILE=/etc/jupyterhub/$NAME.pid
SCRIPTNAME=/etc/init.d/$NAME

# Exit if the package is not installed
[ -x "$DAEMON" ] || exit 0

# Read configuration variable file if it is present
[ -r /etc/default/$NAME ] && . /etc/default/$NAME

# Load the VERBOSE setting and other rcS variables
. /lib/init/vars.sh

# Define LSB log_* functions.
# Depend on lsb-base (>= 3.2-14) to ensure that this file is present
# and status_of_proc is working.
. /lib/lsb/init-functions

#
# Function that starts the daemon/service
#
do_start()
{
	# Return
	#   0 if daemon has been started
	#   1 if daemon was already running
	#   2 if daemon could not be started
	start-stop-daemon --start --quiet -d "$WORKING_DIR" --pidfile $PIDFILE --exec $DAEMON --test > /dev/null \
		|| return 1
	start-stop-daemon --start --background --make-pidfile --quiet -d "$WORKING_DIR" --pidfile $PIDFILE --exec $DAEMON -- \
		$DAEMON_ARGS \
		|| return 2
	# Add code here, if necessary, that waits for the process to be ready
	# to handle requests from services started subsequently which depend
	# on this one.  As a last resort, sleep for some time.
}

#
# Function that stops the daemon/service
#
do_stop()
{
	# Return
	#   0 if daemon has been stopped
	#   1 if daemon was already stopped
	#   2 if daemon could not be stopped
	#   other if a failure occurred
	start-stop-daemon --stop --quiet --retry=TERM/30/KILL/5 --pidfile $PIDFILE --name $NAME
	RETVAL="$?"
	[ "$RETVAL" = 2 ] && return 2
	# Wait for children to finish too if this is a daemon that forks
	# and if the daemon is only ever run from this initscript.
	# If the above conditions are not satisfied then add some other code
	# that waits for the process to drop all resources that could be
	# needed by services started subsequently.  A last resort is to
	# sleep for some time.
	start-stop-daemon --stop --quiet --oknodo --retry=0/30/KILL/5 --exec $DAEMON
	[ "$?" = 2 ] && return 2
	# Many daemons don't delete their pidfiles when they exit.
	rm -f $PIDFILE
	return "$RETVAL"
}

#
# Function that sends a SIGHUP to the daemon/service
#
do_reload() {
	#
	# If the daemon can reload its configuration without
	# restarting (for example, when it is sent a SIGHUP),
	# then implement that here.
	#
	start-stop-daemon --stop --signal 1 --quiet --pidfile $PIDFILE --name $NAME
	return 0
}

case "$1" in
  start)
	[ "$VERBOSE" != no ] && log_daemon_msg "Starting $DESC" "$NAME"
	do_start
	case "$?" in
		0|1) [ "$VERBOSE" != no ] && log_end_msg 0 ;;
		2) [ "$VERBOSE" != no ] && log_end_msg 1 ;;
	esac
	;;
  stop)
	[ "$VERBOSE" != no ] && log_daemon_msg "Stopping $DESC" "$NAME"
	do_stop
	case "$?" in
		0|1) [ "$VERBOSE" != no ] && log_end_msg 0 ;;
		2) [ "$VERBOSE" != no ] && log_end_msg 1 ;;
	esac
	;;
  status)
	status_of_proc "$DAEMON" "$NAME" && exit 0 || exit $?
	;;
  #reload|force-reload)
	#
	# If do_reload() is not implemented then leave this commented out
	# and leave 'force-reload' as an alias for 'restart'.
	#
	#log_daemon_msg "Reloading $DESC" "$NAME"
	#do_reload
	#log_end_msg $?
	#;;
  restart|force-reload)
	#
	# If the "reload" option is implemented then remove the
	# 'force-reload' alias
	#
	log_daemon_msg "Restarting $DESC" "$NAME"
	do_stop
	case "$?" in
	  0|1)
		do_start
		case "$?" in
			0) log_end_msg 0 ;;
			1) log_end_msg 1 ;; # Old process is still running
			*) log_end_msg 1 ;; # Failed to start
		esac
		;;
	  *)
		# Failed to stop
		log_end_msg 1
		;;
	esac
	;;
  *)
	#echo "Usage: $SCRIPTNAME {start|stop|restart|reload|force-reload}" >&2
	echo "Usage: $SCRIPTNAME {start|stop|status|restart|force-reload}" >&2
	exit 3
	;;
esac

:
```

使用方法

```bash
# jinzhongxu 普通用户 或 root 用户均可使用
service jupyterhub status
service jupyterhub start
service jupyterhub stop

/etc/init.d/jupyterhub status
/etc/init.d/jupyterhub start
/etc/init.d/jupyterhub stop
```

开启成功后，访问网址 http://localhost:8000/wsl 即可打开，输入用户名: jinzhongxu，密码: 该用户登录系统的密码。

# 如果可使用 systemctl

如果系统可以使用 systemctl，则可使用如下配置。创建文件 `/etc/systemd/system/jupyterhub.service`，写入内容如下

```bash
[Unit]
Description=Jupyterhub service
After=syslog.target network.target

[Service]
ExecStart=/usr/local/miniconda/bin/jupyterhub -f /etc/jupyterhub/config.py
WorkingDirectory=/etc/jupyterhub/

[Install]
WantedBy=multi-user.target
```

使用方法如下

```bash
# root 用户使用，普通用户请使用 sudo 
systemctl status jupyterhub.service
systemctl start jupyterhub.service
systemctl stop jupyterhub.service
systemctl enable jupyterhub.service
```

