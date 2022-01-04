---
title: Windows 和 Ubuntu 查看网卡 Mac 地址和硬盘序列号的方法
author: Jinzhong Xu
mathjax: true
date: 2020-06-12 11:02:41
tags:
- windows
- ubuntu
categories: technology
---

# Windows 系统

## Windows 系统查看网卡的 Mac 地址方法

打开 CMD，输入

<!--more-->

```powershell
ipconfig /all
```

找到物理地址

## Windows 系统查看硬盘序列号的方法

打开 CMD，输入

```powershell
wmic diskdrive get serialnumber
```

查看序列号

## Windows 系统初始安装日期

Win + r 打开运行

```bash
cmd /k systeminfo | find "初始安装日期"
```



# Ubuntu 系统

## Ubuntu 系统查看网卡的 Mac 地址方法

打开终端，输入

```shell
ifconfig
```

找到所使用网卡的 ether

或者直接输入

```shell
sudo lshw -c network | grep serial | head -n 1
```

获得正在使用的网卡的 Mac 地址

## Ubuntu 系统查看硬盘的序列号

在 Ubuntu 桌面版中，搜索 disk，打开，找到所使用硬盘的 Serial Number

## Ubuntu 系统查看初始安装日期

打开终端，输入如下命令

```bash
sudo tune2fs -l /dev/sda1 | grep create
```

