---
title: Centos 给用户增加sudo 权限
date: 2020-01-02 20:01:08
tags:
- centos
- linux
categories: technology
---

当给 Centos 增加新用户时，想配置新用户具有sudo权限，可是跟ubuntu上不同，在Ubuntu上可以使用简单的命令

```
usermod -aG sudo jinzhongxu
```

<!--more-->

来给用户jinzhongxu增加sudo权限。但是，在Centos上却行不通，提示 没有sudo组。那么在Centos上如何给用户增加sudo权限呢。经过查看 /etc/sudoers 文件，发现wheel组下面的用户都可以具有sudo 权限，因此，可以经新用户添加到wheel组，使其具有sudo 权限，具体命令(root权限)如下

```bash
usermod -aG wheel jinzhongxu
```

在命令行输入

```bash
sudo yum update
```

可以发现，新用户jinzhongxu已经具有sudo权限了。