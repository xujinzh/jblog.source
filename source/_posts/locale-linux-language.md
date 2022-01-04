---
title: 在 Linux 上设置不同的语言和时区
mathjax: true
author: Jinzhong Xu
date: 2020-12-30 16:40:18
tags:
- linux
- locale
- centos
- debian
- ubuntu
categories:
- technology
- linux
---

Linux 系统语言的设置至关重要，当设置不匹配的语言时，严重影响使用的效率。如在系统上没有设置中文时，大多出现乱码、数字等情况，使得使用的体验感大幅下降。下面介绍在 Debian, CentOS 等系统上设置中文或其他语言的方法。本篇命令以 root 身份运行。

<!--more-->

# Debian 类

```bash
apt update
apt install locale
dpkg-reconfigure locales
# 使用空格选择相应的语言，使用上下键移动光标
# 选择好需要安装的语言后，使用 TAB 键切换到 ok
# 使用上下键选择默认的使用的语言

# 安装成功后，需重启系统
shutdown -r now
# 使用下面命令查看是否生效
locale
# 使用下面命令查看安装的语言
locale -a
```

设置时区

```bash
dpkg-reconfigure tzdata
```



# CentOS 

```bash
yum update
# 查看目前系统语言
localectl status
# 或者通过如下命令也可以查看
cat /etc/locale.conf
# 查看系统中安装了哪些语言
localectl list-locales
# 查看系统中安装了哪些英文语言
localectl list-locales | grep en_
# 设置默认语言为 en_GB.utf8
localectl set-locale LANG=en_GB.utf8
# 查看修改默认语言是否成功
localectl status
# 更过使用帮助
localectl --help
```

设置时区

```bash
timedatectl set-timezone Asia/Shanghai
# 如何知道有那些时区可以设置，可以使用如下命令查看
ls /usr/share/zoneinfo
```



# 参考链接

[How to Set Up System Locale on CentOS 7](https://www.rosehosting.com/blog/how-to-set-up-system-locale-on-centos-7/)