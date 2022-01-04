---
title: Linux端口占用情况查询与释放端口
date: 2019.12.21
tags:
- linux
- port
categories: technology
---

查询特定端口占用情况：

```bash
root@ip-172-26-1-96:/# netstat -ntulp | grep 8080
```

```bash
tcp        0      0 127.0.0.1:8080          0.0.0.0:*               LISTEN      5759/filebrowser
```

结果最后一位分别是进程id/进程名，使用如下方法杀死进程：

<!--more-->

```bash
root@ip-172-26-1-96:/# kill 5759
```

再次查看端口得到释放

```bash
root@ip-172-26-1-96:/# netstat -ntulp | grep 8080
```