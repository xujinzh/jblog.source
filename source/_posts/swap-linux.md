---
title: 为Centos 或Ubuntu添加交换分区swap
author: Jinzhong Xu
date: 2020-03-10 13:09:57
tags:
- swap
- centos
- ubuntu
categories: technology
---

Centos 或者 Ubuntu的交换分区swap，类似于Windows系统的虚拟内存，能够在系统内存不足时，利用一部分硬盘空间虚拟出内存空间，解决内存不足的问题。下面分别给出如何为Centos 和 Ubuntu增加交换分区。

<!--more-->

# Centos 添加交换分区

```bash
sudo dd if=/dev/zero of=/swapfile count=2048 bs=1MiB
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo cp /etc/fstab /etc/fstab.bak
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
echo 'vm.swappiness=10' >> /etc/sysctl.conf
echo 'vm.vfs_cache_pressure=50' >> /etc/sysctl.conf
```

这里count=2048，表示2G的交换分区。

# Ubuntu添加交换分区

```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo cp /etc/fstab /etc/fstab.bak
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
echo 'vm.swappiness=10' >> /etc/sysctl.conf
echo 'vm.vfs_cache_pressure=50' >> /etc/sysctl.conf
```

这里的2G表示分配交换分区的硬盘大小为2G。

# 查看交换分区情况

```bash
free -h
htop
```

以上两个命令都可以查看是否成功创建了交换分区。

# 增减swap分区的大小

以centos为例，将swap分区从2G减小到1G。一般swap分区的大小适合设置为内存memory的2倍。下面分步骤给出运行代码演示，以root用户操作。

1. 停用swap

```bash
swapoff /swapfile
```



2. 删除旧swapfile，并重新创建一个新的，这里count设置为你需要的大小，以M为单位

```bash
rm -f /swapfile && dd if=/dev/zero of=/swapfile count=1024 bs=1MiB
```



3. 安装swap

```bash
mkswap /swapfile
```



4. 激活swap

```bash
swapon /swapfile
```



5. 最好将swapfile的访问权限设置为root私享

```bash
chmod 600 /swapfile
```

通过以上步骤设置完后，就可以使用命令free -h查看交换分区的大小了。