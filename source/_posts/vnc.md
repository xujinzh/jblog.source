---
title: 通过VNC远程连接Ubuntu18.04的桌面
author: Jinzhong Xu
mathjax: true
date: 2020-08-20 11:09:01
tags:
- vnc
- ubuntu
categories: 
- technology
- ubuntu
---

VNC 是虚拟网络计算（Virtual Network Computing），是一项允许使用远程帧缓冲协议（RFB）远程控制另一台计算机的技术。在这里，将介绍如何在 Ubuntu 18.04 LTS  和 CentOS 上安装和配置 VNC 服务器。本文默认使用用户 root

<!--more-->

# 安装 VNC 服务

```shell
# debian or ubuntu
apt update
apt -y install tightvncserver
# centos
yum update -y
yum install -y tigervnc-server xorg-x11-fonts-Type1
```

# 安装桌面环境

```shell
# debian or ubuntu
apt install xfce4 xfce4-goodies
# centos
yum install xfce4 xfce4-goodies -y


# 或者 ubuntu 安装 gnome
apt -y install ubuntu-gnome-desktop
sudo systemctl start gdm
sudo systemctl enable gdm
# centos
yum -y groups install "GNOME Desktop"
```

# 配置 VNC 服务

## 设置密码

```shell
vncpasswd
```

##  启动服务

```shell
# 在 5901 端口
vncserver :1

# 如果使用 vncserver :2 则在 5902 端口
```

## 关闭服务

```shell
vncserver -kill :1
```

## 设置 VNC 服务桌面环境

```shell
sudo vim ~/.vnc/xstartup
# 添加如下到末尾
startxfce4 &
# gnome-session &
```

## 设置语言

```bash
apt-get update
#apt-get install locales
dpkg-reconfigure locales
# 空格键是选择(如下面两个语言)，选择完了后Tab切换到OK，再ENTER确认
en_US.UTF-8
zh_CN UTF-8 UTF-8
# 如果进入后访问网页，中文还有显示为方块则需要装字体
apt-get install ttf-wqy-zenhei
# 然后重启
shutdown -r now
```



# 连接远程 VNC 桌面

首先，先在远程主机上打开 VNC 服务，更多参数如下

```shell
vncserver :1 -geometry 800x600 -depth 24 
# Display number [1]
# Screen resolution [800×600]
# Color depth [24]
```

其次，本地连接，推荐使用 **MobaXterm** 或 REALVNC ，连接方法如下

```shell
# 打开 Sessions，选择VNC
# 填写 Remote hostname or IP address
# 选择端口 5901，如果使用 vncserver :2,则端口是5902
# 输入设置的密码，登录
```

此外，也可以使用 Linux 的 VNC 查看器，需要安装

```shell
ssh <username>@<vnsserverip>  -C  -L 5901:127.0.0.1:5901
sudo apt install tigervnc-viewer
```

# 配置自启 VNC 服务

```shell
vncserver -kill :1
sudo vim /etc/systemd/system/vncserver@.service
```

添加如下内容

```shell
[Unit]
Description=Start TightVNC server at startup
After=syslog.target network.target

[Service]
Type=forking
User=jinzhongxu
Group=jinzhongxu
WorkingDirectory=/home/jinzhongxu

PIDFile=/home/jinzhongxu/.vnc/%H:%i.pid
ExecStartPre=/usr/bin/vncserver -kill :%i > /dev/null 2>&1
ExecStart=/usr/bin/vncserver -depth 24 -geometry 1280x800 :%i
ExecStop=/usr/bin/vncserver -kill :%i

[Install]
WantedBy=multi-user.target
```

然后，开启自启

```shell
sudo systemctl daemon-reload
sudo systemctl enable --now vncserver@1
systemctl status vncserver@1
```

# 扩展阅读

[How To Install and Configure VNC Server on Ubuntu 18.04 LTS](https://computingforgeeks.com/how-to-install-vnc-server-on-ubuntu-18-04-lts/)