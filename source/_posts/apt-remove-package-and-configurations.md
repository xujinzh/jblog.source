---
title: 在 Ubuntu 上删除软件和清理配置文件
mathjax: true
author: Jinzhong Xu
date: 2021-10-27 16:39:33
tags:
- ubuntu
categories:
- technology
- ubuntu
---

在 Ubuntu（或其他 Debian 类系统）上安装卸载软件常常都是通过包管理器 apt (或 apt-get) 来实现的。通过 apt remove packagename 和 apt purge packagename 都可以卸载软件，那么它们都有什么区别呢，它们都做了什么呢？本篇进行简单介绍。

<!--more-->

# remove

使用命令 

```bash
apt remove packagename 
```

确实能够卸载软件，但是，它会保留软件的配置文件信息。当下次再次安装该软件时，将会检索到保留的配置文件，安装时将会跳过软件的配置环节。

# dpkg

使用命令 `dpkg --list packagename` 可以查看软件的信息，如

```bash
$ dpkg --list python
Desired=Unknown/Install/Remove/Purge/Hold
| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
||/ Name                     Version           Architecture      Description
+++-========================-=================-=================-======================================================
ii  python                   2.7.15~rc1-1      amd64             interactive high-level object-oriented language
```

```python
not-installed    - The package is not installed on this system
config-files     - Only the configuration files are deployed to this system
half-installed   - The installation of the package has been started, but not completed
unpacked         - The package is unpacked, but not configured
half-configured  - The package is unpacked and configuration has started but not completed
triggers-awaited - The package awaits trigger processing by another package
triggers-pending - The package has been triggered
installed        - The packaged is unpacked and configured OK
```

当使用命令 `apt remove packagename` 卸载包后，再使用 `dpkg --list packagename` 可以看到出现 "rc"，而不是 "ii"，表示卸载（remove）和保留配置文件（config-files）。

那么如何清理配置文件呢？或者在卸载时同时清空配置文件信息，这样下次安装时能够重新安装并重新配置，使用下面的命令。

# purge

使用命令

```bash
apt purge packagename
```

能够同时卸载软件和清理软件配置。

在 Ubuntu 上，很多人会使用 apt autoremove 命令，此命令能够清理多余的软件包，但是，却将配置文件保留在系统中，应慎用。建议以后多使用命令

```bash
apt purge packagename
apt --purge remove packagename
```

当然，如果想要保留配置文件，建议直接使用 

```bash
apt remove packagename
```

# 参考链接

1. [Removing packages and configurations with apt-get](http://bencane.com/2014/08/18/removing-packages-and-configurations-with-apt-get/)
2. [关于apt-get remove 与 apt-get purge](https://www.jianshu.com/p/f6176973b56f)

