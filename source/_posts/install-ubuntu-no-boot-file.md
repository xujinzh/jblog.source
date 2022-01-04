---
title: 安装 Ubuntu 时出现 no boot file found
date: 2020-01-02 18:31:35
tags:
- ubuntu
- windows
categories: technology
---

今天一个师弟安装 Ubuntu 时重启发现无法正常进入刚安装好的 Ubuntu 系统，从系统盘启动时出现 no boot file found。他也很疑惑，明明已经正确把 ubuntu 系统安装到固态硬盘上了，为什么不能正常进入呢？他就来问我怎么解决。我第一想法就是 boot 启动设置引导项的问题，把之前的 UEFI 设置成 legacy ，仍然遇到同样的问题。我就在想，是不是系统盘的引导分区不正确。经过一系列的操作终于完成了Ubuntu系统的安装和正常启动。我的方法如下：

<!--more-->

## 使用Windows启动盘

首先将Windows10 启动盘插入光驱，进入安装Windows10程序，然后在安装系统之前，按住 SHIFT + F10 进入cmd

## 转换硬盘的引导分区

在cmd中输入下列命令，把GPT（GUID Partition Table）引导分区转化为MBR（Master Boot Record）引导分区。

```bash
diskpart
list disk
select disk 0
clean
convert mbr
```

## 设置boot启动格式

重启系统，拿出光盘，进入boot系统，设置光盘启动为第一启动项，设置成legacy格式

## 重装Ubuntu系统

插入Ubuntu系统盘到光驱，进行正常安装Ubuntu流程

