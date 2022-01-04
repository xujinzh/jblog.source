---
title: SSH远程连接Centos出现cannot change locale (UTF-8)
author: Jinzhong Xu
date: 2020-03-12 23:42:56
tags:
- centos
- ssh
categories: technology
---

Centos是一个优秀的安全的linux发行版，但是，当我使用Mac终端SSH远程连接Centos服务器时却出现如下警告：

-bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory=

这里给出解决该问题的方法：

<!--more-->

在Centos服务器上，进入

```bash
sudo vim /etc/environment
```

添加如下代码

```shell
LANG=en_US.utf-8
LC_ALL=en_US.utf-8
```

再次通过SSH连接时就不会出现上述警告了。

