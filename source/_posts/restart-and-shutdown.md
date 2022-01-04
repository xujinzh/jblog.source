---
title: Linux 关机和重启命令
author: Jinzhong Xu
mathjax: true
date: 2020-03-27 14:33:12
tags:
- shutdown
- reboot
categories: technology
---

重新启动Linux系统和关闭Linux系统推荐使用shutdown命令。

下面分别介绍重新Linxu系统的命令：shutdown, reboot, init等命令，以及关机命令：shutdown, halt, init, poweroff

<!--more-->

# shutdown

该命令需要root权限，且功能比较强大，像reboot，halt等都是基于该命令。

shutdown命令比较安全，使用该命令后，它会在系统关闭之前给系统上的所有登录用户一条警告。同时该命令搭配不同的参数，可以设置不同的重启操作。

该命令的一般格式：<font size=3 color=red> shutdown [选项] [时间] [警告信息] </font>

选项的含义:

  - k: 并不真正关机而只是发出警告信息给所有用户
  - r: 关机后立即重新启动
  - h: 关机后不重新启动
  - f: 快速关机重启动时跳过fsck
  - n: 快速关机不经过init 程序
  - c: 取消一个已经运行的shutdown

# reboot

reboot 命令比较粗暴，重启系统时直接删除所有进程，不是平稳安全的关闭它们。但简单粗暴的它可以快速关闭系统。容易造成其他用户的数据丢失。所以建议在单用户模式下才使用reboot命令，在多用户模式下使用shutdown命令。

# init

init命令主要用于切换系统的运行级别，切换是立即完成的。

init 0：表示将运行级别切换为0，表示关机；

init 6：表示将运行级别切换为6，表示重启。

# halt

该命令类似于reboot，是相对简单的关机命令。但实质上是调用了shutdown -h，但它有如下一些恐怖的参数

- f: 没有调用shutdown而强制关机或重启
- i: 关机或重新启动之前，关掉所有的网络接口
- p: 关机时调用poweroff，此选项为缺省选项

# poweroff

**poweroff命令**用来关闭计算机操作系统并且切断系统电源。它有如下参数

- n：关闭操作系统时不执行sync操作；
- w：不真正关闭操作系统，仅在日志文件“/var/log/wtmp”中；
- d：关闭操作系统时，不将操作写入日志文件“/var/log/wtmp”中添加相应的记录；
- f：强制关闭操作系统；
- i：关闭操作系统之前关闭所有的网络接口；
- h：关闭操作系统之前将系统中所有的硬件设置为备用模式。

# 如何重启

下面总结一些常用的重启命令:

立即安全的重启（推荐的命令），可以考虑参数r与reboot对应

```bash
shutdown -r now
```

10分钟后重启

```bash
shutdown -r +10
```

立刻重启

```bash
reboot
init 6
```

# 常用关机命令

下面总结一些常用的关机命令：

立即安全的关机（推荐的命令），可以考虑参数h与halt对应

```bash
sudo shutdown -h now
```

10分钟后关机

```bash
sudo shutdown -h +10
```

晚上18：30关机

```bash
sudo shutdown -h 18:30
```

在未来某天某时关机

```bash
echo "shutdown -h +5" | at 10:05am 2022-01-19
```

关机时广播指定信息

```bash
sudo shutdown -h +20 "System Upgrade"
```

立刻关机

```bash
halt
init 0
poweroff
sudo shutdown -h +0
```

取消定时关机

```bash
sudo shutdown -c "Canceling System Upgrade"
```

# 参考链接

1. [How to use Linux Shutdown Command with Examples](https://phoenixnap.com/kb/linux-shutdown-command)
