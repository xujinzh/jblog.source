---
title: 利用 nginx 反向代理为 jupyterlab 配置二级网址页面
mathjax: true
author: Jinzhong Xu
date: 2020-11-09 14:46:55
tags:
- python
- nginx
- jupyter
- jupyterlab
- linux
categories:
- technology
- python
---

当服务器配置好 Python, Jupyterlab 后，访问总是通过 8888 端口，出于某些原因，想要为其配置二级页面，这样当想要更改端口时只需要修改一下配置，而不需要再次记忆到底是打开了哪个端口，只需要记住二级页面 web 地址就可以。本篇不介绍如何配置 Jupyterlab，而是假设已经配置好了 Jupyterlab，且申请了域名，直接说如何通过 nginx 配置 Jupyterlab.

<!--more-->

# 安装 nginx

```bash
sudo apt update
sudo apt install nginx
sudo systemctl status nginx.service
sudo systemctl start nginx.service
sudo systemctl enable nginx.service
```

# 配置 Jupyterlab

```bash
# 生成 jupyterlab 配置文件
jupyter notebook --generate-config
# 修改配置
vim .jupyter/jupyter_notebook_config.py
# 修改如下内容
c.NotebookApp.base_project_url = '/jupyter'
c.NotebookApp.ip = '0.0.0.0'
# 或者修改如下内容
c.NotebookApp.base_url = '/jupyter'
c.NotebookApp.ip = '0.0.0.0'
```

```bash
#  配置 nginx
cat /etc/nginx/nginx.conf
# 修改如下内容
server {
        listen       80;
        server_name  www.googj.com;
        location /jupyter {
            proxy_pass          http://127.0.0.1:8888;
            proxy_set_header    Host $host;
            proxy_set_header    X-Real-Scheme $scheme;
            proxy_set_header    X-Real-IP $remote_addr;
            proxy_set_header    X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_http_version  1.1;
            proxy_set_header    Upgrade $http_upgrade;
            proxy_set_header    Connection "upgrade";
            proxy_read_timeout  120s;
            proxy_next_upstream error;
        }
    }

```

重启服务器后，在网页上访问地址：http://www.googj.com/jupyter (这里是样例地址)，就可以打开 Jupyterlab.

注意，上面假设已经配置了 Jupyterlab 自启，如果没有可以访问我的其他博文配置。并假设域名已经指向服务器IP地址。

# 参考链接

1. [Jupyter Notebook在nginx中无法配置为二级页面的解决办法](https://www.byincd.com/bobjiang/article-0161/)

2. [nginx反向代理jupyterlab配置](http://blog.wangcaimeng.top/2020/01/31/nginx%E5%8F%8D%E5%90%91%E4%BB%A3%E7%90%86jupyterlab%E9%85%8D%E7%BD%AE/)

   