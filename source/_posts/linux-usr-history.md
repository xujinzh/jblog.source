---
title: Linux 系统下 usr 目录由来
mathjax: true
author: Jinzhong Xu
date: 2020-12-09 13:52:34
tags:
- linux
- usr
categories:
- technology
- linux
---

在 Linux（或Unix） 系统下，存放二进制文件的目录一般用 bin 表示，如 /bin, /usr/bin, /usr/local/bin, /opt/bin 等等，它们有什么区别？

<!--more-->

在文章: [Understanding the bin, sbin, usr/bin , usr/sbin split](http://lists.busybox.net/pipermail/busybox/2010-December/074114.html) 中可以知道，Linux 目录结构是历史造成的。

1969年，Ken Thompson 和 Dennis Ritchie 在小型机 PDP-7上发明了 Unix。1971年，他们将主机升级到 PDP-11。当时使用的存储盘 RK05 容量大约是1.5MB，随着使用，根目录（/）下的操作系统越来越大，无法装下。于是增加了第二张盘 RK05，专门储存用户（user）程序，取名挂载点为 /usr，两张盘的结构完全一样，如在第一张盘或根目录下的 /bin, /sbin, /lib, /tmp... 在第二张盘 /usr 目录下也有。但是，随着时间继续，储存的程序越来越多，就需要第三张盘，取名挂载点 /home，用于储存 /usr 里面用户的数据。

随着系统的发展，各个目录的含义进一步明确：

1. /：存放系统程序，也就是 At&t 开发的 Unix 程序。

　2. /usr：存放 Unix 系统商（比如 IBM 和 HP ）开发的程序。

　3. /usr/local：存放用户自己安装的程序。

　4. /opt：在某些系统，用于存放第三方厂商开发的程序，所以取名为 option，意为"选装"。

参考链接：

1. [Unix目录结构的来历](http://www.ruanyifeng.com/blog/2012/02/a_history_of_unix_directory_structure.html)
2. [Understanding the bin, sbin, usr/bin , usr/sbin split](http://lists.busybox.net/pipermail/busybox/2010-December/074114.html)