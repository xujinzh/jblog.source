---
title: 查看Linux系统电脑的cpu和内存
author: Jinzhong Xu
date: 2020-03-04 11:46:11
tags:
- linux
- ubuntu
categories: technology
---

CPU情况和内存情况

cpu配置和内存配置决定了电脑的主要性能

电脑cpu配置和内存配置决定了电脑的主要性能。Windows系统非常简单的通过任务管理器查看，而Linux系统更偏向于利用命令行，下面介绍如何通过命令行有效的查看Linux系统的cpu和内存的情况。

<!--more-->

# 查看cpu情况

## 物理cpu个数

```bashb
cat /proc/cpuinfo | grep "physical id" | sort | uniq | wc -l
```

## 每个物理cpu的核心数

```bashb
cat /proc/cpuinfo | grep "cpu cores" | uniq
```

## 总的逻辑cpu个数

```
cat /proc/cpuinfo | grep "processor" | wc -l
```

## 查看cpu型号

```bashb
cat /proc/cpuinfo | grep name | cut -f2 -d: | uniq -c
```

# 查看内存信息

## 查看内存运行情况

```bash
free -h
```

或者使用如下命令

```bash
cat /proc/meminfo
```

# 使用htop查看

命令htop不仅能够显示的给出cpu和内存的多少信息，而且可以给出它们的消耗情况，给出系统的运行压力情况，运行时间情况等信息。所以，htop是一个非常有用的命令。如何使用该命令，直接在终端输入

```bash
htop
```

就会显示上述信息。想要退出，可以按一下键盘上的q