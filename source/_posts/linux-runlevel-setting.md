---
title: Linux 系统的运行级别及设置
mathjax: true
author: Jinzhong Xu
date: 2021-04-02 14:44:02
tags:
- linux
categories:
- technology
- linux
---

Linux 系统默认有多种运行级别，如 Ubuntu 系统有 7 种运行级别，通过设置不同的运行级别，可以对计算机进行访问控制（如单用户模式，多用户模式等）、关机、重启、关闭图形界面、打开图形界面等。下面分别对这些运行级别进行介绍，并给出如何使用，如关闭图形界面，节省计算资源。

<!--more-->

#  runlevel

查看当前计算机的运行级别

```bash
$ runlevel
N 3
# 运行级别是 3，说明是多用户控制台模式
```

通过命令可以查看各个运行级别信息

```bash
$ man runlevel
Table 1. Mapping between runlevels and systemd targets
       ┌─────────┬───────────────────┐
       │Runlevel │ Target            │
       ├─────────┼───────────────────┤
       │0        │ poweroff.target   │
       ├─────────┼───────────────────┤
       │1        │ rescue.target     │
       ├─────────┼───────────────────┤
       │2, 3, 4  │ multi-user.target │
       ├─────────┼───────────────────┤
       │5        │ graphical.target  │
       ├─────────┼───────────────────┤
       │6        │ reboot.target     │
       └─────────┴───────────────────┘
```

- 运行级别 0：系统停机状态，系统默认运行级别不要设为 0，否则不能正常启动；

- 运行级别 1：单用户工作状态，具有 root 权限，用于系统维护，禁止远程登陆；

- 运行级别 2：多用户状态 ( 没有NFS，不能联网 )；

- 运行级别 3：完全的多用户状态 ( 有 NFS，能联网 )，登陆后进入控制台命令行模式；

- 运行级别 4：系统未使用，保留；

- 运行级别 5：X11 控制台，登陆后进入图形 GUI 模式；

- 运行级别 6：系统正常关闭并重启，默认运行级别不能设为 6，否则不能正常启动；

因此，普通用户一般较多使用运行级别 5 或者 3.

# 切换运行级别

关闭计算机

```bash
init 0
```

重启计算机

```bash
init 6
```

查看默认启动级别

```bash
systemctl get-default
```

设置默认启动为多用户控制台模式 3

```bash
sudo systemctl set-default multi-user.target
```

从控制台模式切换到图形界面

```bash
# 不需要输入用户名和密码
startx

# 需要输入用户名和密码
systemctl isolate graphical.target  # 可以通过systemctl isolate multi-user.target再切换回命令行模式

# 需要输入用户名和密码
init 5   #可以通过 init 3 再切换回命令行模式

# 快捷键方法
可使用 Ctrl + Alt + F1~6 进行切换，Ctrl + Alt + 1 为图形界面
```

# 参考链接

1. [Linux系统7个运行级别(runlevel)详解](https://zhuanlan.zhihu.com/p/25816300)
2. [Linux 的Run level介绍](https://blog.csdn.net/liu1055087125/article/details/48381007)
3. [Linux(一)：运行级别查询及切换](https://blog.csdn.net/yiifaa/article/details/77266951)
4. [Linux运行级别及切换方法](https://blog.csdn.net/qq_21453783/article/details/100251461)