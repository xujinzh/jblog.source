---
title: Virtualbox 以NAT模式创建虚拟机并通过SSH连接
author: Jinzhong Xu
date: 2020-03-07 15:44:46
tags:
- virtualbox
- ssh
categories: technology
---

Virtualbox 创建虚拟机时，如果按照 NAT 方式进行创建，则各虚拟机之间可以进行 SSH 连接，虚拟机也可以 SSH 连接宿主机，但是，宿主机想要连接虚拟机却需要一些设置才可以连接。想知道虚拟机桥接、NAT、host-only之间的区别，可查看我之前的文章：[虚拟机网络模式 NAT 桥接 Host-Only](https://xujinzh.github.io/2019/12/20/virtual-machine-network-nat-bridge-hostonly/)	

<!--more-->

# 设置端口映射

打开设置 --- Network --- NAT --- 高级 --- 端口映射，添加如下内容

| Protocol | Host IP   | Host Port | Guest IP  | Guest Port |
| -------- | --------- | --------- | --------- | ---------- |
| TCP      | 127.0.0.1 | 10002     | 10.0.2.15 | 22         |

这里假设虚拟机的 ip 地址是 10.0.2.15，可以通过在虚拟机运行命令 ifconfig 查看

# SSH连接

在宿主机运行命令SSH连接虚拟机

```bash
ssh jinzhongxu@127.0.0.1 -p 10000
```

# SCP 传输文件

如果想从宿主机向虚拟机传送文件，可以使用如下命令

```bash
scp -P 10000 jdk* 127.0.0.1:/home/jinzhongxu/.
```

