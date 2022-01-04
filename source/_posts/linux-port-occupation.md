---
title: Linux 端口占用与释放
author: Jinzhong Xu
mathjax: true
date: 2020-07-21 11:05:25
tags:
- linux
- port
categories: technology
---

Linux 端口占用会导致某些想使用该端口运行的程序无法成功，比如，启动Zookeeper时默认2181端口占用，这时候除了修改配置文件的端口号外，也可以将Linux服务器的端口释放掉供Zookeeper使用。下面介绍，如何查看端口释放被占用以及如何释放端口，一般查看端口占用有两个命令可以使用，分别是 lsof 和 netstat 命令，释放端口除了正常关闭占用端口的程序外就是直接 kill 掉占用端口的程序 pid

<!--more-->

# 查看端口占用

## lsof

lsof(list open files)是一个列出当前系统打开文件的工具。

lsof 查看端口占用语法格式：

```bash
lsof -i:port

# 例子
lsof -i:2181
```

其他更多用法：

```
lsof -i:8080：查看8080端口占用
lsof abc.txt：显示开启文件abc.txt的进程
lsof -c abc：显示abc进程现在打开的文件
lsof -c -p 1234：列出进程号为1234的进程所打开的文件
lsof -g gid：显示归属gid的进程情况
lsof +d /usr/local/：显示目录下被进程开启的文件
lsof +D /usr/local/：同上，但是会搜索目录下的目录，时间较长
lsof -d 4：显示使用fd为4的进程
lsof -i -U：显示所有打开的端口和UNIX domain文件
```

## netstat

**netstat -tunlp** 用于显示 tcp，udp 的端口和进程等相关情况

```vbscript
-t (tcp) 仅显示tcp相关选项
-u (udp)仅显示udp相关选项
-n 拒绝显示别名，能显示数字的全部转化为数字
-l 仅列出在Listen(监听)的服务状态
-p 显示建立相关链接的程序名
```

netstat 查看端口占用语法格式：

```bash
netstat -tunlp | grep port

# 例子
netstat -tunlp | grep 2181
```

# 释放端口占用

## kill 

在查到端口占用的进程后，如果你要杀掉对应的进程可以使用 kill 命令：

```bash
kill PID

# 或者当无法杀掉时，强制杀掉
kill -9 PID
```

像一些大数据框架服务，如Zookeeper, Kafka, Flink, Storm等，可以使用命令 jps 查看相应的 pid

# 更多阅读

本篇参考：[Linux 查看端口占用情况](https://www.runoob.com/w3cnote/linux-check-port-usage.html)

