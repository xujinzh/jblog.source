---
title: 使用 filebrowser 和 Nginx 在 VPS 搭建云盘
mathjax: true
author: Jinzhong Xu
date: 2020-11-19 09:36:35
tags:
- cloud
- filebrowser
- nginx
- vps
categories:
- technology
- ubuntu
---

个人云盘能够方便存储个人文档，特别是没有上传下载速度限制，可以灵活扩展容量，具有更好的私密性。当具有远程服务器（如VPS）时，可以使用 [filebrowser](https://filebrowser.org/) 搭建个人云盘。结合 [Nginx](https://nginx.org/en/linux_packages.html#Debian) 实现个性化网页快速访问。下面分别介绍如何在远程服务器上安装 Nginx, filebrowser, 以及他们的配置。本篇以 Debian 10 为例，Ubuntu 系统类似， CentOS 系统需要切换相应命令，但一般是将 apt 更改为 yum.

<!--more-->

# 安装 Nginx

```bash
sudo apt update
sudo apt install nginx
```

# 安装 filebrowser

```bash
curl -fsSL https://filebrowser.org/get.sh | bash
```

# 配置 filebrowser

```bash
# 创建配置数据库
filebrowser -d .filebrowser.db config init
# 设置监听地址
filebrowser -d .filebrowser.db config set --address 0.0.0.0
# 设置监听端口，需要打开端口 6666
filebrowser -d .filebrowser.db config set --port 6666
# 设置网址根路径
filebrowser -d .filebrowser.db config set --baseurl /cloud
```

设置服务器自动启动命令

```bash
sudo vim /etc/systemd/system/filebrowser.service
# 添加如下代码
[Unit]
Description=filebrowser daemon
After=network.target

[Service]
Type=simple
ExecStart=filebrowser -d /home/xiangyin/.filebrowser.db
User=xiangyin
Group=xiangyin
WorkingDirectory=/home/xiangyin/files-path/
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

```bash
# 查看状态
sudo systemctl status filebrowser.service
# 启动 filebrowser
sudo systemctl restart filebrowser.service
# 设置开机自启
sudo systemctl enable filebrowser.service
# 查看是否设置开机自启成功
sudo systemctl list-unit-files| grep filebrowser
# 如果对service文件进行修改再次启动，请运行如下命令，重载服务
sudo systemctl daemon-reload
```

此时，已经可以访问网页：http://ip:6666 查看目录 /home/xiangyin/files-path/ 下的文件

# 配置 Nginx

```bash
sudo vim /etc/nginx/nginx.conf
# 在 server 下添加如下内容
location /cloud {
            client_max_body_size 2048m;
            proxy_read_timeout   86400s;
            proxy_send_timeout   86400s;
            proxy_set_header     X-Forwarded-Host $host;
            proxy_set_header     X-Forwarded-Server $host;
            proxy_set_header     X-Real-IP $remote_addr;
            proxy_set_header     Host $host;
            proxy_set_header     X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_http_version   1.1;
            proxy_redirect       off;
            proxy_set_header     Upgrade $http_upgrade;
            proxy_set_header     Connection "upgrade";
            proxy_pass           http://127.0.0.1:6666;
        }
# 注意这里的 /cloud 需要同配置 filebrowser baseurl 一致
```

如果申请了域名，可以如下设置

```bash
server {
        listen       80;
        server_name  www.baigoo.com;
        root /home/xiangyin/html;
        index index.php index.html index.htm;
}
# 其中 www.baigoo.com 是域名，/home/xiangyin/html 是静态网页文件放置目录
# 注意，请打开 80、443 端口
```

设置 nginx 服务

```bash
sudo systemctl daemon-reload
sudo systemctl status nginx.service
sudo systemctl restart nginx.service
sudo systemctl enable nginx.service
```

之后，就可以访问网址：http://www.baigoo.com/cloud 打开个人云盘了。注意，进入后设置强密码。

# 参考链接

1. [filebrowser.org](https://filebrowser.org/)
2. [nginx.org](https://nginx.org/en/)
3. [在服务器上搭建个人网盘](https://blog.csdn.net/xujinzh/article/details/98040033)
4. [使用filebrowser搭建私人云盘](https://diannaobos.com/post/828.html)