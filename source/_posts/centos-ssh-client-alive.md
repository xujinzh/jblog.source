---
title: Centos远程SSH保持在线方法
author: Jinzhong Xu
mathjax: true
date: 2020-03-21 09:55:58
tags:
- ssh
- centos
categories: technology
---

Centos 安全可靠，但是远程通过SSH连接Centos服务器时，总是会出现过一段时间不用或隔夜第二天断开的情况，这是由于Centos的SSH服务配置项设置导致的，可以修改相应的配置项来保证Centos通过SSH连接时不自动断开。

<!--more-->

# 修改配置项

打开配置文件

```bash
sudo vim /etc/ssh/sshd_config
```

修改如下两项

```shell
#ClientAliveInterval 0
#ClientAliveCountMax 3
```

为如下

```shell
ClientAliveInterval 60
ClientAliveCountMax 3
```

参数说明：

&emsp; ClientAliveInterval 指定了服务器端向客户端请求消息的时间间隔，默认值为0，表示不发送。设置ClientAliveInterval = 60 表示每分钟发送一次，客户端进行相应，保持在线不断开。

&emsp; ClientAliveCountMax  表服务器发出请求后客户端没有响应的次数达到一定值就会自动断开。使用默认值3即可，因为正常情况下，不会不响应。

# 重启SSH服务

重启SSH服务

```bash
sudo systemctl restart sshd
```

或者如下命令

```bash
service sshd restart
```

重启后，就使设置生效了。即正常情况下，远程SSH连接不会断开。

参考博文：

1. [CentOS 7 SSH连接超时自动断开解决方案](https://blog.csdn.net/libaineu2004/article/details/83857779).
2. [CentOS下解决SSH自动断开办法](https://blog.csdn.net/moliyiran/article/details/54809090).