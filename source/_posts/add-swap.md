---
title: 给服务器添加交换分区swap 和开启 bbr
date: 2019-12-20 19:51:06
tags:
- ubuntu
- swap
categories: technology
---

有时候，购买的云服务器不会自动给创建交换分区，如何自己建立交换分区呢，使用下面命令创建交换分区，可以更改大小，比如1G更改为2G，一般交换分区是内存的2倍足矣。本篇以 Debian 系统为例，建议以 root 身份运行本篇中的命令。

<!--more-->

#  添加交换分区

```bash
sudo swapon --show
free -h
df -h
sudo fallocate -l 1G /swapfile
ls -lh /swapfile
sudo chmod 600 /swapfile
ls -lh /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo swapon --show
free -h
sudo cp /etc/fstab /etc/fstab.bak
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
cat /proc/sys/vm/swappiness
sudo sysctl vm.swappiness=10
echo 'vm.swappiness=10' >> /etc/sysctl.conf
cat /proc/sys/vm/vfs_cache_pressure
sudo sysctl vm.vfs_cache_pressure=50
echo 'vm.vfs_cache_pressure=50' >> /etc/sysctl.conf
```

使用 top 或 htop 命令，可以查看交换分区是否创建成功。

# 开启 bbr

```bash
echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
sysctl -p
sysctl net.ipv4.tcp_available_congestion_control

# 查看是否开启成功
lsmod | grep bbr
```

