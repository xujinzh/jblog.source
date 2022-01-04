---
title: 利用 jupyterhub 搭建多用户 jupyterlab 开发环境
mathjax: true
author: Jinzhong Xu
date: 2021-02-20 09:57:44
tags:
- python
- jupyterlab
- jupyterhub
categories:
- technology
- python
---

使用 jupyterlab 开发 python 程序非常方便，但是只能单用户访问。想要多用户共用服务器使用 jupyterlab 可以采用 jupyterhub 实现。下面演示在 CentOS 上搭建 jupyterhub 实现多用户访问 jupyterlab。本篇所有命令均使用 root 用户运行，jupyterhub 版本小于 2.0.0，对于 jupyterhub 2.0.0 的部署方法请参看[这篇](https://xujinzh.github.io/2021/12/21/jupyterhub-2-multiuser/)。

<!--more-->

# 安装 Miniconda

```bash
yum install wget -y
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
chmod +x Miniconda*.sh
# 安装在 /usr/local/miniconda 目录下
./Miniconda*.sh
rm -rf Miniconda*.sh
# 将 miniconda 命令目标添加到系统环境变量中，使得所有用户均可访问
echo 'export PATH=/usr/local/miniconda/bin:$PATH' >> /etc/profile
source /etc/profile
# 创建 miniconda 用户组，使得包含在该组下的用户均可使用 miniconda 里的 python
groupadd miniconda
chgrp -R miniconda /usr/local/miniconda
chmod 770 -R /usr/local/miniconda
usermod -a -G miniconda root
usermod -a -G miniconda jinzhongxu
# 查看某用户所属组可以使用 
id root
id jinzhongxu
groups
# 安装 jupyterhub, jupyterlab 以及其他常用的包
pip install --upgrade pip jupyterhub jupyterlab pandas matplotlib youtube_dl numba boost scipy numpy sympy scikit-learn
# 安装配置代码格式化工具
pip install jupyterlab_code_formatter;jupyter server extension enable jupyterlab_code_formatter;pip install black isort
```

# 安装 npm, nodejs

```bash
yum update -y
yum remove nodejs npm -y
yum install epel-release -y
yum install psmisc vim htop zsh git nload curl bzip2 util-linux-user gcc npm -y
curl -sL https://rpm.nodesource.com/setup_14.x | sudo bash -
yum install nodejs -y
```

# 配置 jupyterhub

```bash
yum install npm -y
npm install -g configurable-http-proxy
eventlog_dir=`find / -name eventlog.py`
sed -i "s/ruamel.yaml/ruamel_yaml/g" ${eventlog_dir}
jupyterhub --generate-config
mkdir -p /etc/jupyterhub
mv jupyterhub_config.py /etc/jupyterhub/config.py
echo "c.JupyterHub.base_url = '/jupyterhub'" >> /etc/jupyterhub/config.py
echo "c.JupyterHub.ip = '0.0.0.0'" >> /etc/jupyterhub/config.py
echo "c.Spawner.default_url = '/lab'" >> /etc/jupyterhub/config.py
echo "c.PAMAuthenticator.open_sessions = False" >> /etc/jupyterhub/config.py
echo "c.LocalAuthenticator.create_system_users = True" >> /etc/jupyterhub/config.py
echo 'c.Authenticator.admin_users = set(["jinzhongxu"])' >> /etc/jupyterhub/config.py
echo "c.JupyterHub.admin_access = True" >> /etc/jupyterhub/config.py
echo "c.Spawner.cmd = ['/usr/local/miniconda/bin/jupyterhub-singleuser']" >> /etc/jupyterhub/config.py
wget https://www.dropbox.com/s/vak1nsajt7s8muj/jupyterhub.service?dl=0 -O jupyterhub.service
mv jupyterhub.service /etc/systemd/system/jupyterhub.service
systemctl daemon-reload
systemctl restart jupyterhub.service
systemctl enable jupyterhub.service
```

# 配置 nginx

```bash
# 安装 nginx 不再介绍，然后，修改配置文件
vim  /etc/nginx/nginx.conf
# 在
server {
        listen       80;
        server_name  www.vvv.com;
        root /usr/share/nginx/html;
        index index.php index.html index.htm;
# 下面添加如下内容：
location /jupyterhub {
            proxy_pass           http://127.0.0.1:8000;
            proxy_set_header     Host $host;
            proxy_set_header     X-Real-Scheme $scheme;
            proxy_set_header     X-Real-IP $remote_addr;
            proxy_set_header     X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_http_version   1.1;
            proxy_set_header     Upgrade $http_upgrade;
            proxy_set_header     Connection 'upgrade';
            proxy_read_timeout   120s;
            proxy_next_upstream  error;
        }

```

上面所有都配置好后，访问 https://www.vvv.com/jupyterhub 即可登录。

更多搭建方式（如 docker）可参考 https://github.com/jupyterhub/jupyterhub

# 增加新用户

在网页上的以用户 jinzhongxu 登录 jupyterhub后，默认进入 jupyterlab，点击 File --> Hub Control Panel 进入 jupyterhub 管理后台，点击 admin 即可管理增加用户，开启服务，关闭服务，关闭 Hub 等。

点击增加用户 username 后，根据上面的配置默认会在系统上创建该用户及家目录。但此时还不能使用过 jupyterlab 服务，需要管理员登录 shell，将新创建的用户增加到 miniconda 组中，命令如下：

```bash
usermod -a -G miniconda username
# 同时给该用户增加密码
passwd username
```

此时，该用户即可登录 https://www.vvv.com/jupyterhub 了，用户名和密码就是该用户登录 shell 的用户名（username）和密码。

# 禁止某用户登录

一种方法是使用命令将该用户的密码删除

```bash
passwd -d username
```

另一种方法是，将该用户从 miniconda 组中剔除

```bash
# 此命令仅从 miniconda 组中删除用户 username，但不会永久地从系统中删除用户
deluser username miniconda
# 或者
gpasswd -d username miniconda
```



# 参考文献

1. [github:jupyterhub](https://github.com/jupyterhub/jupyterhub)

2. [How to set a multiuser Jupyterlab server with Jupyterhub](https://www.hatarilabs.com/ih-en/how-to-set-a-multiuser-jupyterlab-server-with-jupyterhub-in-windows-with-docker)

3. [jupyterhub 安装教程](https://blog.csdn.net/luoluonuoyasuolong/article/details/88526526)

4. [JupyterHub 部署与应用指南](https://sthsf.github.io/wiki/Linux%20Tricks/JupyterHub%20%E9%83%A8%E7%BD%B2%E4%B8%8E%E5%BA%94%E7%94%A8%E6%8C%87%E5%8D%97.html)

5. [单机多用户 JupyterLab 环境搭建](https://gist.github.com/tanbro/a94bfa4a552381f599e7e6b551ccadcf)

6. [Linux下多用户Anaconda环境的安装与卸载](https://blog.csdn.net/codedancing/article/details/103936542)

7. [Linux: 删除用户密码/禁止登录](https://www.cnblogs.com/sheldonxu/archive/2012/05/22/2513036.html)

8. [如何在 Ubuntu 上为用户授予和移除 sudo 权限](https://zhuanlan.zhihu.com/p/57883153)

   

   