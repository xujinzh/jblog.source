---
title: Windows 10 子系统 wsl 关闭和重启电脑方法
mathjax: true
author: Jinzhong Xu
date: 2020-09-28 09:56:05
tags:
- windows
- wsl
categories:
- technology
- ubuntu
---

使用 Windows 10 的子系统能够非常方便的体验到 Linux 系统的简洁美（方便开发），同时也能够体验到 Windows10 图形界面的直接美（方便使用常用软件）。Windows 10 结合 WSL 能够在一定程度上消除了装载虚拟机的时间。但 WSL 只有终端界面没有图形界面，那么如何通过 WSL 关闭 Windows 10呢？这里以 Ubuntu-18.04 为例介绍。

<!--more-->

# System32

首先需要通过终端进入 System32 目录下

```bash
cd /mnt/c/Windows/System32
```

# Shutdown

其次使用 shutdown.exe 进行管理计算机开机、重启、休眠等

```bash
# 取消关机
./shutdown.exe -a           
# 关机
./shutdown.exe -s   
# 强行关闭应用程序
./shutdown.exe -f            
# 控制远程计算机
./shutdown.exe -m computer-name
# 显示远程关机图形用户界面，但必须是shutdown的第一个参数
./shutdown.exe -i
# 注销当前用户
./shutdown.exe -l
# 关机并重启，默认1分钟后关机
./shutdown.exe -r
# 设置关机倒计时
./shutdown.exe -s -t time
# 休眠，休眠后需要按一下开机键才能唤醒主机
./shutdown.exe -h
```

例子如下

```bash
# 30秒后关机
./shutdown.exe -s -t 30
# 如果不想关机，可以取消关机
./shutdown.exe -a
# 关机前先关闭软件
./shutdown.exe -f
# 1秒后关机并重启，近似实时关机重启
./shutdown.exe -r -t 1
```

如果想打开 Windows 10 上面的软件，同样也可以，如打开 teamviewer

```bash
cd /mnt/c/Program\ Files\ \(x86\)/TeamViewer
./TeamViewer.exe
# 打开后，可以按 ctrl + c 返回 terminal，不会影响已经打开的 teamviewer
```

参考

[win10系统如何CMD中进行电脑关机或重启](https://jingyan.baidu.com/article/48b558e30df4947f39c09a42.html)