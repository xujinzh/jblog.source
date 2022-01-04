---
title: 使用命令行更改苹果电脑的 hostname
mathjax: true
author: Jinzhong Xu
date: 2021-12-12 13:30:24
tags:
- mbp
- hostname
- terminal
categories:
- technology
- mac
---

更改苹果电脑名称可在系统便好设置里的共享编辑更改，但重启后终端上却没有发生变化，这篇介绍 Mac Book Pro 更改终端 hostname 方法。

<!--more-->

打开终端，运行如下命令，其中 `newhostname`为你想设置的新名字。

```bash
scutil --set ComputerName "newhostname"
scutil --set LocalHostName "newhostname"
scutil --set HostName "newhostname"
```

查看设置后的名字。

```bash
scutil --get HostName
# 或者
hostname
```

