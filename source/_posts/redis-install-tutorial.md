---
title: Redis 安装教程
author: Jinzhong Xu
mathjax: true
date: 2020-06-19 10:35:08
tags:
- redis
- linux
categories: technology
---

Redis 是一款内存型高性能的key-value的No-SQL数据库，性能高，比较受欢迎。下面介绍一下再Ubuntu 18.04系统上编译安装 redis  稳定版本（次版本）的方法，并配置如何开机自启以及查看运行情况命令。

<!--more-->

## 下载redis

redis的最新稳定版本6.0.5可以从其[官网下载](https://redis.io/download)，这里根据公司需求，要下载次稳定版本，就是下载6.0.4，下面介绍如何下载。在下载官网页面下，找到 How to verify files for integrity，点击GitHub repository 的 [redis-hashes](https://github.com/antirez/redis-hashes/blob/master/README) ，最下面的倒数第二行就是次稳定版本的链接：http://download.redis.io/releases/redis-6.0.4.tar.gz

使用命令下载：

```shell
wget http://download.redis.io/releases/redis-6.0.4.tar.gz
```

## 安装redis

如下安装就是参考官网方法：

```shell
tar -xzf redis-6.0.4.tar.gz
cd redis-6.0.4
make
```

此时，就已经编译好redis，可以使用了。使用方法如下

```shell
# 开启redis服务
src/redis-server

# 使用redis客户端连接
src/redis-cli
```

但是，如上方法对于本机使用还可以，如果想远程链接、其他目录启动与连接、设置密码、开机自启等都无法实现。这些都需要对redis配置文件进行修改。下面介绍一下。

## 配置redis

### 远程连接

修改redis配置文件

```shell
# 找到下面配置，并注释掉
# bind 127.0.0.1

# 设置保护模式为no
protected-mode no

# 设置监控模式为systemd，注意该设置与下面的开机自启对应，不设置启动不起来
supervised systemd

# 设置数据库保存位置
 dir /var/redis
 
 # 设置自己的密码，这里设置为redis
 requirepass redis
```

创建数据库目录，并赋给读写权限

```shell
sudo adduser --system --group --no-create-home redis
sudo mkdir -p /var/redis
sudo chown redis:redis /var/redis
sudo chmod 770 /var/redis
```

在另一台Ubuntu18.04上安装redis客户端工具

```shell
sudo apt install redis-tools
```

通过redis客户端远程连接redis数据库

```shell
redis-cli -h 1.14.1.14 -p 6379 -a redis
```

如果忘记输入密码，可以在redis命令行里再次输入

```shell
redis-cli -h 1.14.1.14 -p 6379

# 进入redis后，再输入密码
$ redis-cli -h 1.14.1.14 -p 6379
1.14.1.14:6379> keys *
(error) NOAUTH Authentication required.
1.14.1.14:6379> AUTH redis
OK
1.14.1.14:6379> keys *
(empty list or set)
1.14.1.14:6379>

```

### 开机自启

```shell
sudo vim /lib/systemd/system/redis.service
```

我这里是把redis文件夹放在了/home/jinzhongxu/Documents目录下

```shell
[Unit]
Description=Redis In-Memory Data Store
After=network.target

[Service]
User=redis
Group=redis
ExecStart=/home/jinzhongxu/Documents/redis-6.0.4/src/redis-server /home/jinzhongxu/Documents/redis    -6.0.4/redis.conf
ExecStop=/home/jinzhongxu/Documents/redis-6.0.4/src/redis-cli shutdown
Restart=always

[Install]
WantedBy=multi-user.target
```

```shell
# 查看状态
sudo systemctl status redis.service

# 关闭服务
sudo systemctl stop redis.service

# 开启服务
sudo systemctl start redis.service

# 开机自启
sudo systemctl enable redis.service
```

### 查看运行状态

除了上面的查看状态方法，还可以通过如下方法进行查看

```shell
# 查看redis进程
ps -ef | grep redis
# 或者以树状图方式查看
pstree -ph | grep redis

# 检测监听端口
netstat -nlt | grep 6379
```

### Ubuntu14.04 安装redis

在Ubuntu14.04.5上安装redis的大概步骤如下，在gcc版本满足编译redis版本的情况下，使用如下即可。如果gcc版本低，而ubuntu14.04.5可以联网，可以联网升级gcc，然后再编译安装redis

```shell
wget http://download.redis.io/releases/redis-6.0.4.tar.gz
tar xzf redis-6.0.4.tar.gz
cd redis-6.0.4
make
sudo make install
cd utils
sudo ./install_server.sh
# 默认该步骤会自动设置用户、开机启动等

# 查看状态并启动
service redis-server status
service redis-server stop
service redis-server start
```

但是，在不能联网的Ubuntu14.04.5上安装redis-6.0.4最大的问题就是gcc版本太低问题，默认版本是4.8.4，但是，需要5.3以上，那么就必须安装一个高版本的gcc，然后是用高版本的gcc编译安装redis

### 下载gcc

在网址[Introduction to GCC](http://www.linuxfromscratch.org/blfs/view/8.0/general/gcc.html)，下载[GCC-6.3.0](http://ftpmirror.gnu.org/gcc/gcc-6.3.0/gcc-6.3.0.tar.bz2) ，然后拷贝到ubuntu14.04.5上。

### 安装gcc 6.3.0

```shell
tar -xjf gcc-6.3.0.tar.bz2
cd gcc
# configure gcc时可能会报错，需要安装如下三个包
# apt install libgmp-dev libmpfr-dev libmpc-dev

# 如果只需要配置64位的，可以使用如下方法
./configure --disable-multilib
make
sudo make install
```

### 安装redis-6.0.4

```shell
tar -xzf redis-6.0.4.tar.gz
cd redis-6.0.4
# 可以使用 gcc --version查看刚刚安装好的gcc目录，我这边安装在/usr/local/bin/gcc
make CC=/usr/local/bin/gcc
make install
cd utils
sudo ./install_server.sh
```

```shell
sudo service redis-6370 status
sudo service redis-6370 stop
sudo service redis-6370 start
```

