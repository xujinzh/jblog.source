---
title: 使用 xrdp 远程连接 Ubuntu
mathjax: true
author: Jinzhong Xu
date: 2021-04-07 09:43:33
tags:
- xrdp
- ubuntu
categories:
- technology
- ubuntu
---

我这里有一个需求就是使用 Windows10 远程连接 Ubuntu 的桌面系统，但是，Ubuntu 服务器在机房且没有连接显示器，只能通过终端 SSH 远程连接。然而，处于一些需求，需要远程连接，而 Teamviewer 在远程服务器没有连接显示器的情况下会出现灰屏，无法使用。因此，使用如下方法来解决。

<!--more-->

# 安装 xrdp

```bash
sudo apt update && sudo apt -y upgrade
sudo apt install xrdp
```

# 安装 xfce

```
sudo apt install -y xfce4
sudo apt install -y xfce4-goodies
```

# 配置 xrdp

```bash
sudo cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bak
sudo sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/max_bpp=32/#max_bpp=32\nmax_bpp=128/g' /etc/xrdp/xrdp.ini
sudo sed -i 's/xserverbpp=24/#xserverbpp=24\nxserverbpp=128/g' /etc/xrdp/xrdp.ini
echo xfce4-session > ~/.xsession

# 编辑启动方式为 xfce4
sudo vim /etc/xrdp/startwm.sh

# test -x /etc/X11/Xsession && exec /etc/X11/Xsession
# exec /bin/sh /etc/X11/Xsession

# xfce
startxfce4
```

# 启动 xrdp

```bash
sudo systemctl status xrdp.service
sudo systemctl stop xrdp.service
sudo systemctl start xrdp.service
sudo systemctl enable xrdp.service

# 或者
/etc/init.d/xrdp status
/etc/init.d/xrdp stop
/etc/init.d/xrdp start
```

# 远程连接

在 Windows10 上打开远程桌面连接，输入 Ubuntu 服务器的 IP 和 端口，如下示例

```bash
1.2.3.4:3389
```

然后，输入SSH 连接时的用户名和密码即可连接。