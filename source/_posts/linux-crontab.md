---
title: Linux 定时任务 crontab
mathjax: true
author: Jinzhong Xu
date: 2021-03-19 11:18:55
tags:
- linux
- crontab
- ubuntu
categories:
- technology
- linux
---

在 Linux 上执行定时任务具有便利性，可以使用命令 crontab 来实现。如因为服务器资源紧张需要错峰使用，想要在晚上才进行具有较大运行压力的任务时，或者对于执行任务有具体时间要求时。本篇以 Ubuntu 20.04 为例进行介绍。

<!--more-->

# crontab 

可以使用如下命令查看 crontab

```bash
➜  ~ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed

# 每小时的第17分钟执行命令： cd / && run-parts --report /etc/cron.hourly
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly

# 每天6点25分执行命令： test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )

# 每周日的6点47分执行命令： test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )

# 每个月1号6点52分执行命令：test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )

# 每一分钟执行一次命令
* *  * * *   root    cd / && run-parts --report /etc/cron.hourly

# 每3分钟执行一次命令
*/3  *  * * *   root    cd / && run-parts --report /etc/cron.hourly

# 每天1点第1，3分钟执行一次命令
1,3  1  * * *   root    cd / && run-parts --report /etc/cron.hourly

```

创建一个定时任务

```bash
crontab -e
# 所有任务在 /var/spool/cron 中
```

在以下添加按照上面规则定义任务命令

```bash
# Edit this file to introduce tasks to be run by cron.
#
# Each task to run has to be defined through a single line
# indicating with different fields when the task will be run
# and what command to run for the task
#
# To define the time you can provide concrete values for
# minute (m), hour (h), day of month (dom), month (mon),
# and day of week (dow) or use '*' in these fields (for 'any').
#
# Notice that tasks will be started based on the cron's system
# daemon's notion of time and timezones.
#
# Output of the crontab jobs (including errors) is sent through
# email to the user the crontab file belongs to (unless redirected).
#
# For example, you can run a backup of all your user accounts
# at 5 a.m every week with:
# 0 5 * * 1 tar -zcf /var/backups/home.tgz /home/
#
# For more information see the manual pages of crontab(5) and cron(8)
#
# m h  dom mon dow   command
1 1 1 * * shutdown -r now # 每月1号1点1分重启系统
```

查看定时任务

```bash
crontab -l
```

删除定时任务

```bash
crontab -r
```

# cron

cron 是 crontab 命令的守护进程，在后台运行，负责每分钟监测是否有预定的任务需要执行。

查看 cron 守护进程是否运行

```bash
➜  ~ systemctl status cron.service
● cron.service - Regular background program processing daemon
     Loaded: loaded (/lib/systemd/system/cron.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2021-03-17 17:10:58 CST; 1 day 19h ago
       Docs: man:cron(8)
   Main PID: 417 (cron)
      Tasks: 1 (limit: 1160)
     Memory: 6.8M
     CGroup: /system.slice/cron.service
             └─417 /usr/sbin/cron -f

Warning: some journal files were not opened due to insufficient permissions.

```

开启 cron 守护进程

```bash
➜  ~ systemctl start cron.service
```

关闭 cron 守护进程

```bash
➜  ~ systemctl stop cron.service
```

重启 cron 守护进程

```bash
➜  ~ systemctl restart cron.service
```

设置开机启动 cron 守护进程

```bash
➜  ~ systemctl enable cron.service
```

# 参考链接

1. [crontab](https://blog.csdn.net/qq_41755706/article/details/112801266)

2. [Linux学习之CentOS(十二)--crontab命令的使用方法](https://www.cnblogs.com/xiaoluo501395377/archive/2013/04/06/3002602.html)

3. [linux定时运行命令脚本——crontab](https://blog.csdn.net/ithomer/article/details/6817019)

4. [Linux Crontab 定时任务](https://www.runoob.com/w3cnote/linux-crontab-tasks.html)

5. [linux 定时器 crontab 实例 计划任务 定时任务](https://blog.csdn.net/whatday/article/details/106834657)

   

   