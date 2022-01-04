---
title: Linux 系统中的开机自启命令简单介绍
author: Jinzhong Xu
mathjax: true
date: 2020-06-12 11:13:14
tags:
- linux
- ubuntu
categories: technology
---

Linux 系统可以通过命令行，有效简便快捷的启动程序、设置开机自启的程序等，并且，往往有多个命令可以达到这一效果。但是，需要我们了解这些命令并知悉它们之间的区别，下面主要简单的总结一下，以下代码以 Ubuntu18.04 为例

<!--more-->

# 启动程序命令

## /etc/init.d/appname

常用方法：

```shell
# 查看ssh服务状态
/etc/init.d/ssh status   

# 启动ssh服务，注意需要sudo权限
sudo /etc/init.d/ssh start

# 关闭ssh服务
sudo /etc/init.d/ssh stop
```

/etc/init.d/ 其实是一个目录，里面存放的都是系统启动时需要运行或关闭的命令，这些命令常常通过软连接，连接到各级启动级别的文件夹中，如 /etc/rc3.d/，通过下面介绍的命令 update-rc.d 和 systemctl 可以设置开机启动或关闭

## service

常用方法：

```shell
# 查看防火墙状态
service ufw status

# 关闭防火墙，注意sudo权限
sudo service ufw stop

# 启动防火墙
sudo service ssh start
```

service 是一个运行 System V 的 init script，它其实是运行 /etc/init.d 和  /{lib,run,etc}/systemd/system 中的命令或程序，对于 /etc/init.d/ 就是等同于上面介绍的命令 /etc/init.d/appname

## systemctl

常用方法：

```shell
# 查看zabbix-agent运行状态
systemctl status zabbix-agent.service

# 关闭zabbix-agent
systemctl stop zabbix-agent.service

# 启动zabbix-agent
systemctl start zabbix-agent.service
```

该命令非常强大，不仅可以启动程序，还可以设置程序开机自启等，不过在 Ubuntu 14.04等之前的系统中未有该命令。在 Centos、Redhat 等系统中，设置程序开机自启的有 chkconfig 等，不过，现在逐渐被抛弃。功能上 systemctl = service + chkconfig

# 设置开机自启命令

## update-rc.d

常用方法：

```shell
# 取消开机启动zabbix-agent，从/etc/rcN.d中清除到/etc/init.d/zabbix-agent链接
update-rc.d -f zabbix-agent remove

# 设置默认启动级别的zabbix-agent开机自启
update-rc.d zabbix-agent defaults

# 设置zabbix-agent开机自启
update-rc.d zabbix-agent enable
```

## chkconfig

常用方法：

```shell
# 设置开机启动zabbix-agent
chkconfig --add zabbix-agent

# 设置开机不启动zabbix-agent
chkconfig --del zabbix-agent
```

在 Debian、Ubuntu上无该命令，在Centos、RedHat上可以使用

## systemctl

常用方法：

```shell
# 开机启动ssh服务
systemctl enable ssh.socket

# 开机不启动ssh服务
systemctl disable ssh.socket

# 查看zabbix-agent是否开机启动
systemctl list-unit-files | grep zabbix-agent
```

