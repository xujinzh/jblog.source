---
title: Zabbix 监控系统安装与简单设置
author: Jinzhong Xu
mathjax: true
date: 2020-06-08 17:19:46
tags:
- zabbix
- ubuntu
categories: technology
---

Zabbix 是一款开源免费的企业级监控软件，原作者是 Alexei Vladishev，编程语言是C（Server 端）和PHP（frontend），跨平台，可以用于集群网络监控、管理系统等。下面简单记录一下Zabbix服务的安装和利用Zabbix监控Linux服务器。服务器采用Ubuntu 18.04，把Zabbix Server 安装在box0，负责监控box1和box2等

<!--more-->

# 安装Zabbix

访问网址：[Zabbix](https://www.zabbix.com/download?zabbix=5.0&os_distribution=ubuntu&os_version=20.04_focal&db=mysql&ws=apache) ，选择Install from Packages

## 选择系统（Choose your platform）

这里选择 ZABBIX VERSION: 5.0 LTS, OS DISTRIBUTION: Ubuntu, OS VERSION: 18.04 (Bionic), DATABASE: MySQL, WEB SERVER: Apache

## 安装Zabbix 服务（Install and configure Zabbix server for your platform）

*注意：所有命令以<font color='dd0000'> root </font>身份运行*

### 安装Zabbix仓库（Install Zabbix repository）

```shell
wget https://repo.zabbix.com/zabbix/5.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.0-1+bionic_all.deb
dpkg -i zabbix-release_5.0-1+bionic_all.deb
apt update
```

### 安装Zabbix服务，前端和代理 （Install Zabbix server, frontend, agent）

```shell
apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-agent
```

### 建立初始化数据库（Create initial database）

*注意：'<font color='dd0000'>password</font>' 修改成自己喜欢的密码*

```shell
mysql -uroot -p
# 输入秘密

mysql> create database zabbix character set utf8 collate utf8_bin;
mysql> create user zabbix@localhost identified by 'password';
mysql> grant all privileges on zabbix.* to zabbix@localhost;
mysql> quit;

zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix
```

### 配置数据库（Configure the database for Zabbix server）

编辑 /etc/zabbix/zabbix_server.conf，修改成前面一步设置的密码

```shell
DBPassword=password
```

### 配置PHP前端（Configure PHP for Zabbix frontend）

*注意：这里需要校对时间，方法如下：*

```shell
date
apt install ntpdate
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
ntpdate us.pool.ntp.org
date
```

编辑  /etc/zabbix/apache.conf, 找到时区，并取消注释和修改时区

```shell
php_value date.timezone Asia/Shanghai
```

### 启动Zabbix 服务（Start Zabbix server and agent processes）

```shell
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2
```

### 配置Zabbix前端（Configure Zabbix frontend）

打开网址：http://box0/zabbix，配置Zabbix.

*注意：密码为前面设置的MySQL数据库的Zabbix用户的密码；配置后，使用用户名：Admin，密码: <font color='dd000'>password</font>*

## Zabbix Agent 安装

下面开始依次在 box1 和 box2 上安装需要被监控的Linux服务器上的Zabbix Agent，这个比较简单，基本步骤同Server，不过需要配置一下参数。这里以 box1 为例，box2 类似

### 下载安装源并安装Zabbix Agent

```shell
wget https://repo.zabbix.com/zabbix/5.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.0-1+bionic_all.deb
dpkg -i zabbix-release_5.0-1+bionic_all.deb
apt install zabbix-agent
```

### 设置时区同Zabbix Server

```shell
apt install ntpdate
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
ntpdate us.pool.ntp.org
```

### 设置开机自启动

```shell
update-rc.d zabbix-server defaults

# centos 方法如下
chkconfig zabbix-agent on
```

### 设置开机不启动防火墙

```shell
update-rc.d ufw remove

# centos 方法如下
chkconfig iptables off
```

### 配置Zabbix Agent 参数

```shell
vim /etc/zabbix/zabbix_agentd.conf
```

修改

（*注意：Hostname=box1，不然，zabbix-agent日志会报错找不到hostname，通过 tail /var/log/zabbix-agent/zabbix_agentd.log查看*）

```
Server=box0
ServerActive=box0
Hostname=box1
```

### 重启Zabbix Agent并设置开机自启

```shell
/etc/init.d/zabbix-agent status
/etc/init.d/zabbix-agent stop
/etc/init.d/zabbix-agent start
# 设置开机自启
update-rc.d zabbix-agent defaults
# 或者使用如下命令设置
systemctl enable zabbix-agent
# 查看是否设置成功开机自启
systemctl list-unit-files | grep enabled | grep zabbix
```

类似设置 box1 etc.

# 配置 Hosts

点击 Configuration --> Hosts --> Create host --> Host name(ip or hostname，这里输入box1) --> Visible name(Zabbix界面上可见名字) --> Groups(选择 Linux servers) --> Interfaces，Agent（IP address：box1）

# 配置 Templates

点击 Templates --> Link new templates --> (输入linux) Templates OS Linux by Zabbix agent --> Add



点击 Monitoring-->Hosts，可以查看添加的服务器，选择右边的Graphs，可以查看系统运行情况

类似设置 box1 etc.