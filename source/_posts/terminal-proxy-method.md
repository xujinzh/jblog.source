---
title: 终端代理方法
mathjax: true
author: Jinzhong Xu
date: 2021-04-02 15:19:01
tags:
- terminal
- proxy
categories:
- technology
- linux
---

Linux 系统上终端的使用非常频繁，当需要使用终端进行快速下载软件时（如 conda, pip 命令），对终端进行代理显得非常重要。本篇以 Debian 类系统和 Mac 为例。

<!--more-->

# proxychains

安装和配置如下

```bash
sudo apt update
sudo apt-get install proxychains
sudo vim /etc/proxychains.conf
# 在最后的 ProxyList 下面设置：（根据个人的参数进行）

socks5    127.0.0.1    1080
```

使用方法如下（在需要的命令前加上 `proxychains` 即可）

```bash
proxychains curl cip.cc
```

# **privoxy** 

安装和配置如下

```bash
# Debian 安装方法
sudo apt update
sudo apt install privoxy
sudo vim /etc/privoxy/config

# Mac 安装方法
brew search privoxy
brew install privoxy
sudo vim /usr/local/etc/privoxy/config

# 修改如下，注意最后面的点号
listen-address  127.0.0.1:8118
listen-address  [::1]:8118

forward-socks5t   /               127.0.0.1:1080 .

vim ~/.bashrc
alias setproxy="export ALL_PROXY=127.0.0.1:8118"
alias unsetproxy="unset ALL_PROXY"
```

使用方法如下

```bash
# Debian 启动
sudo /etc/init.d/privoxy start

# Mac 启动
brew services start privoxy

# 使环境变量生效
source ~/.bashrc

# 开启
setproxy

# 检测
curl cip.cc

# 关闭
unsetproxy
```

对终端进行代理设置后，就可以使用 conda 或 pip 从官方源下载最新的包了，而不用使用更新较迟的国内源。