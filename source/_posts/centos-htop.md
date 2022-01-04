---
title: Centos安装htop和htop命令使用
author: Jinzhong Xu
date: 2020-03-10 10:21:15
tags:
- centos
- htop
categories: technology
---

Centos 是一个比较优秀的Linux发行版，在其上可以使用软件包管理工具yum进行软件命令的安装，但是，htop却不能直接使用命令yum install htop来安装，下面介绍如何安装htop，方面我们清楚的查看电脑的运行情况。

<!--more-->

# 首先，需要添加EPEL repo

```bash
sudo yum -y install epel-release
```

# 其次，安装htop

## 搜索htop

```bash
sudo yum search htop
```

## 查看htop信息

```bash
sudo yum info htop
```

## 更新系统

```bash
sudo yum update
sudo yum upgrade
```

注意，在centos中，命令yum update 和yum upgrade有一些小区别：

yum upgrade 会强制删除过时的软件包，这可能是危险的，因为有可能你在使用该软件

yum update 不会删除而会保留它们，这使得yum update 更安全

## 安装htop

```bash
sudo yum install htop
```

# htop命令

## 以无颜色显示

```bash
htop -C
htop --no-color
```

## 以树型显示

```bash
htop -t
htop --tree
```

## 查看某个用户的信息

```bash
htop -u jinzhongxu
htop --user=jinzhongxu
```

## 查看某个进程的信息

```bash
htop -p PID
htop -p 2322, 3999
```

## 查看htop帮助信息

```bash
htop --help
man htop
```

