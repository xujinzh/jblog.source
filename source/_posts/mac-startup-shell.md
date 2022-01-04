---
title: Mac 电脑设置 shell 脚本开机自启
mathjax: true
author: Jinzhong Xu
date: 2021-01-10 17:49:23
tags:
- mac
- shell
categories:
- technology
- mac
---

苹果电脑 Mac 无论是运行流畅度，还是界面都给用户一种非常完美的感受。在 Linux 系统上设置开机自启脚本有很多教程，在 Mac 上如何开机自启 shell 脚本呢，这里介绍一种方法。

<!--more-->

# 编写脚本

这里以脚本 test.sh 为例，该脚本具体如下，即在开机时在用户桌面创建 success 空文件。

```bash
cd ~/Desktop
touch success
```

写入上面内容后，保存为 test.sh，把脚本文件放在用户桌面。

# 设置权限

```bash
sudo chmod 777 test.sh
```

# 修改文件打开方式

在桌面找到 test.sh 文件，右键找到‘显示简介’， 将打开方式修改为‘终端’，共享和权限将所有权限打开，即设置 staff 和 everyone 的权限为读与写。

# 配置开机启动

进入系统偏好设置，找到用户与群组，打开小锁，找到当前用户的登录项，将 test.sh 添加到登录项，并把隐藏选型卡勾选。重启系统即可。

# 参考链接

[mac设置shell脚本开机自启动](https://blog.csdn.net/linhai1028/article/details/80220423)

