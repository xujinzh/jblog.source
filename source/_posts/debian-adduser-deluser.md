---
title: Debian 系统如何增加系统用户和删除用户
date: 2020-01-10 10:35:11
tags:
- debian
- ubuntu
categories: 
- technology
- ubuntu
---

Debian 系统是一各非常优秀的 Linux 操作系统，Ubuntu 是基于 Debian 的衍生版本，具有相同的包管理工具和系统命令。有时候我们需要给 Debian 系统的服务器添加用户，那么可以通过如下命令非常简单的操作。本篇演示默认在 root 用户下操作。

<!--more-->

# 使用 adduser 添加用户

```bash
 adduser jinzhongxu
```

该命令是 Perl 脚本命令，可以交互式的创建用户，需要设置新用户密码，并同时创建同名用户组和家目录。

将该用户添加到 sudo 次组（主组名同用户名）中，使其具有 sudo 权限，命令如下

```bash
usermod -a -G sudo jinzhongxu
```

# 使用 deluser 删除用户

有时候不想再使用用户了，可以删除用户，同时也可以删除该用户所有的文件，使用如下命令

```bash
deluser --remove-all-files jinzhongxu
```

# 其他添加用户命令

其实，除了上面比较方面的 Perl 脚本命令，用更加强大的 Linux 命令 useradd 和 userdel 来添加用户和删除用户，但是，需要的参数比较多，记忆比较复杂，下面列出各参数

## useradd 添加用户

-c 备注：备注出现在 /etc/passwd 第 5 项字段中
        -d 用户主文件夹：指定用户登录所进入的目录，同时赋予用户对该目录的的完全控制权
        -e 有效期：指定用户的有效期限。格式为 YYYY-MM-DD，信息存储在 /etc/shadow 中
        -f 缓冲天数：限定密码过期后多少天，将该用户帐号停用
        -g 主要组：设置用户所属的主要组
        -G 次要组：设置用户所属的次要组，可设置多组
        -M 强制不创建用户主文件夹
        -m 强制建立用户主文件夹，并将 /etc/skel/ 当中的文件复制到用户的根目录下
        -p 密码：设置用户密码
        -s shell：指导用户登录 shell
        -u uid：指定用户标志符 user id，即 uid

### 建一个带有家目录并且可以登录 bash 的用户

```bash
useradd -m -s /bin/bash jinzhongxu
```

### 指定创建用户家目录的路径

```bash
useradd -m -d /home/jinzhongxu jinzhongxu
```

### 创建时把用户加入不同的用户组

```bash
useradd -m -G sudo jinzhongxu
```

### 创建类似于 adduser 方法的用户

```bash
useradd -m -s /bin/bash -d /home/jinzhongxu -G sudo jinzhongxu
```

### 创建用户的同时设置密码

```bash
# 不建议显示设置密码
useradd -m -s /bin/bash -d /home/jinzhongxu -G sudo -p "$(openssl passwd -1 121212)"  jinzhongxu
useradd -m -s /bin/bash -d /home/jinzhongxu -G sudo -p `openssl passwd -1 -salt 'salt' 121212` jinzhongxu
```

## userdel 删除用户

```bash
# 该命令删除用户所有的文件
userdel -r jinzhongxu
```

