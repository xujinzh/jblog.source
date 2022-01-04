---
title: Windows10 子系统 Ubuntu 在硬盘上的路径
date: 2020-01-07 09:37:27
tags:
- windows
- ubuntu
categories: technology
---

Windows10 的 Linux 子系统 Ubuntu 非常好用，能够与Windows10非常融洽的结合使用，既保留了Windows系统优秀的用户交互式，又提供了Linux系统开发的便捷，可谓是集成了两种操作系统的优点于一身。那么，如何互相访问两者的文件呢？下面给出两者相互访问的方法。

<!--more-->

# Ubuntu 访问 Windows 文件

```bash
cd /mnt/c/Users/xujin/Downloads
```

# Windows 访问 Ubuntu 文件

Ubuntu 用户家目录在Windows的如下路径

```windows
E:\Users\xujin\AppData\Local\Packages\CanonicalGroupLimited.Ubuntu18.04onWindows_79rhkp1fndgsc\LocalState\rootfs\home\jinzhongxu
```

