---
title: ssh 出现 Could not resolve hostname flink Name or service not known 的解决方案
date: 2019-12-20 19:12:51
tags:
- flink
- ssh
- big data
categories: technology
---

当使用 ssh 远程连接服务器出现 Could not resolve hostname slave.flink: Name or service not known 时，一种测试通过的解决方案如下：
```shell
sudo vim /etc/hosts
```
添加如下代码，就是使本地计算机能够解析该主机名

<!--more-->

```bash
192.168.66.88 flink
```
这样就可以进行 ssh 连接。

