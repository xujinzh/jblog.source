---
title: Linux 文件和目录的默认权限和特殊权限
mathjax: true
author: Jinzhong Xu
date: 2021-03-17 14:46:36
tags:
- linux
- ubuntu
categories:
- technology
- linux
---

对于 Linux 用户而言，当创建文件或目录时都会自动为该文件或目录分配权限，该权限就是默认权限。对于 Linux 文件和目录，除了具有默认权限外，还有特殊权限。默认权限和特殊权限在使用 Linux 系统时非常重要，特别是和团队开发时尤为重要。本篇就介绍一下如何查看和设置默认权限和特殊权限。以 Ubuntu 20.04 为例进行介绍。

<!--more-->

# 文件默认权限：**umask**

查看用户的默认权限

```bash
➜  ~ umask
002
➜  ~ umask -S
u=rwx,g=rwx,o=rx
```

以 umask 显示的是数字，加上参数 -S (Symbolic) 显示的是字符。含义解释：文件权限有 r, w, x 三个，分别对应数字 4， 2， 1，002 表示创建文件、目录时剔除 w 写权限。如 umask -S 显示的是创建目录时的默认权限，从上面命令可以看到其他用户 o 缺少 w 写权限。值得注意的是，文件默认是没有可执行权限 x 的。即文件的默认最高权限是 -rw-rw-rw-，目录的默认最高权限是 drwxrwxrwx，因此，剔除 w 写权限时，用户新创建的文件的默认权限是 -rw-rw-r--，新创建的目录的默认权限是 drwxrwxr-x，如下示例

```bash
➜  ~ touch test1
➜  ~ mkdir test2
➜  ~ ls -ld test[12]  # -d 表示只显示目录本身，不显示它里面包含的内容；中括号表示中间有个指定的字符，而不是任意字符，任意字符用 *
-rw-rw-r-- 1 jinzhongxu jinzhongxu    0 3月  17 15:30 test1
drwxrwxr-x 2 jinzhongxu jinzhongxu 4096 3月  17 15:30 test2
```

如何修改用户的默认权限

```bash
# 如果想要用户创建的新文件或目录的群组和其他人没有写权限，可以设置如下，即剔除用户的写权限
umask 022
```

# 文件特殊权限：**SUID, SGID, SBIT**

对于 Linux 系统中有一个目录 /tmp 它的权限如下

```bash
➜  ~ ls -ld /tmp
drwxrwxrwt 12 root root 4096 3月  17 15:05 /tmp
```

可以看到它的权限中有一个 t，熟悉该目录的人应该知道，/tmp 目录下我们可以自由创建内容，也可以查看别人在该目录下创建的文件和目录，但是，我们不能删除别人创建的文件和目录。同时，别人也不能删除我们自己创建的文件和目录。这一切都要归功于特殊权限。

​	又如

```bash
➜  ~ ls -l /usr/bin/passwd
-rwsr-xr-x 1 root root 68208 5月  28  2020 /usr/bin/passwd
➜  ~ ls -l /etc/shadow
-rw-r----- 1 root shadow 1229 3月  14 21:38 /etc/shadow
```

在 /etc/shadow 中存储的是所有用户的密码，但只有 root 用户可以查看和修改。但是，有些人可能会疑惑了，明明自己可以修改自己用户的密码啊，使用 passwd 直接修改就可以了，不需要通知 root 进行修改。这是为什么呢？其实，这也要归功于 /usr/bin/passwd 可执行文件特殊权限 s，那么这些特殊权限符号代表什么含义呢，如何进行设置呢？比如，使用 iftop 命令需要输入密码，如何不输入密码直接使用呢？

## Set UID

当 s 出现在文件拥有者的 x 权限上时，称为 Set UID，简称为 SUID 的特殊权限。关于该特殊权限有如下几个特点：

1. SUID 权限仅对二进制程序有效，如 passwd, iftop；
2. 执行者对于该程序需要具有执行权限；
3. 本权限仅在执行该程序的过程中有效；
4. 执行者将具有该程序拥有者的权限；

当普通用户使用 passwd 执行修改密码的时候，因为 passwd 拥有特殊权限，因此，普通用户能够使用该命令，且命令执行期间拥有 root 的权限修改 /etc/shadow 文件

## Set GID

当 s 出现在群组的 x 上时，称为 Set GID，简称为 SGID，它有如下特点：

1. SGID 对二进制程序有用；
2. 程序执行者对于该程序来说，需具备 x 的权限；
3. 执行者在执行的过程中将会获得该程序群组的支持；

## Sticky Bit

与 Set UID 和 Set GID 不同，Sticky Bit 只针对目录有效。简称为 SBIT，其对目录的作用是：

1. 当使用者对于此目录具有 w, x 权限时，即具有写入的权限；
2. 当使用者在该目录下创建文件或目录时，仅有自己与 root 才有权利删除该文件；

举例来说，我们的 /tmp 本身的权限是 drwxrwxrwt， 在这样的权限内容下，任何人都可以在 /tmp 内新增、修改文件，但仅有该文件/目录创建者与 root 能够删除自己的目录或文件。

## 设置特殊权限

和读写执行一样，特殊权限也对应有数字：

- 4 为 SUID
- 2 为 SGID
- 1 为 SBIT

如下示例进行特殊权限设置

```bash
➜  ~ touch test
➜  ~ ls -l test
-rw-rw-r-- 1 jinzhongxu jinzhongxu 0 3月  17 16:02 test
➜  ~ chmod 4664 test
➜  ~ ls -l test
-rwSrw-r-- 1 jinzhongxu jinzhongxu 0 3月  17 16:02 test
```

我们发现出现的是大 S，而不是小 s，这是为什么呢？因为文件 test 本身缺乏 x 可执行权限，所以增加特殊权限也为无法执行该文件，大 S 表示空，即无用的特殊权限。想要使用有用的特殊权限，需要我们的文件本身具有执行权限，如下：

```bash
➜  ~ chmod +x test
➜  ~ ls -l test
-rwsrwxr-x 1 jinzhongxu jinzhongxu 0 3月  17 16:02 test
➜  ~ chmod 6775 test
➜  ~ ls -l test
-rwsrwsr-x 1 jinzhongxu jinzhongxu 0 3月  17 16:02 test
➜  ~ chmod u+s,g+s,o+t test
➜  ~ ls -l test
-rwsrwsr-t 1 jinzhongxu jinzhongxu 0 3月  17 16:02 test
```

可见这样就给文件附加了特殊权限。

再比如

```bash
chmod 4755 /usr/sbin/iftop
ll /usr/sbin/iftop
	-rwsr-xr-x 1 root root 68408 9月   5  2019 /usr/sbin/iftop*
```

这样普通用户就不需要输入密码就可以使用命令 iftop 了。

# 参考链接

鸟哥的 LINUX 私房菜 基础学习篇（第四版）