---
title: Ubuntu14.04机器上安装 Zabbix 监控
author: Jinzhong Xu
mathjax: true
date: 2020-06-12 11:50:56
tags:
- zabbix
- ubuntu
categories: technology
---

Zabbix 在 Ubuntu14.04 上使用会收到一些限制，特别是，在本地集群，无法连接互联网时，安装可能会遇到一些坑，下面简单记录一下，并给出解决方案

<!--more-->

1. 参考[官网安装教程](https://www.zabbix.com/download?zabbix=5.0&os_distribution=ubuntu&os_version=14.04_trusty&db=mysql&ws=apache)
2. 下载 wget https://repo.zabbix.com/zabbix/5.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_5.0-1+trusty_all.deb
3. 拷贝 zabbix-release_5.0-1+bionic_all.deb 到需要安装 zabbix server 和 zabbix agent 的Ubuntu14.04服务器上

## 安装 zabbix-server

方法如下：(注意，以root身份运行，它会自动创建zabbix用户和zabbix用户组，无家目录)，假设安装zabbix-server服务器的IP=1.1.1.0

```shell
dpkg -i zabbix-release_5.0-1+bionic_all.deb
apt update
# 不像 ubuntu18.04，需要安装最新的 zabbix-frontend-php、 zabbix-apache-conf
# 结果就是界面比较老旧，虽然选择的是 zabbix5.0.0，其实按照成果后显示 zabbix2.2.2
# 这是因为 ubuntu14.04 系统 php 版本低的问题，ubuntu16.04 也是这样
apt install zabbix-server-mysql zabbix-agent

mysql -uroot -p
# 输入 root 用户密码
# 在创建 mysql 的 zabbix 用户时，请自己替换成自己的 zabbix 数据库密码
mysql> create database zabbix character set utf8 collate utf8_bin;
mysql> create user zabbix@localhost identified by 'password';
mysql> grant all privileges on zabbix.* to zabbix@localhost;
mysql> quit;

zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix

# vim etc/zabbix/zabbix_server.conf，将上面设置的 zabbix 数据库的密码填写下面
DBPassword=password

# 设置开机自启动
service zabbix-server start
update-rc.d zabbix-server enable
service zabbix-agent restart
update-rc.d zabbix-agent enable
service apache2 restart
update-rc.d apache2 enable

# 打开网址 http://server_ip_or_name/zabbix，安装设置 zabbix Web
```

遇到的问题：

1. apt install zabbix-server zabbix agent 出错？

   解决方法：更新 /etc/apt/sources.list；或者 apt-get autoremove -f、dpkg --configure -a；

   ```shell
   # 或者可能需要添加如下内容到 /etc/apt/sources.list
   
   I just wanted to say that this got me on the right track. I had to add the following lines to the /etc/apt/sources.list
   
   ###### Ubuntu Main Repos
   
   deb http://us.archive.ubuntu.com/ubuntu/ bionic main universe
   deb-src http://us.archive.ubuntu.com/ubuntu/ bionic main universe
   
   ###### Ubuntu Update Repos
   
   deb http://us.archive.ubuntu.com/ubuntu/ bionic-security main universe
   deb http://us.archive.ubuntu.com/ubuntu/ bionic-updates main universe
   deb-src http://us.archive.ubuntu.com/ubuntu/ bionic-security main universe
   deb-src http://us.archive.ubuntu.com/ubuntu/ bionic-updates main universe
   ```

   

2. 使用mysql创建zabbix用户时出错？

   解决方法：以root用户进入mysql

   ```mysql
   drop user admin@localhost;
   flush privileges;
   ```

   然后，再重新创建zabbix数据库，并分配权限；注意，打开mysqld服务

3. 将模板数据库sql语句写入zabbix数据库时，无相应模板（zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix）？

   解决方法：下载[zabbix源文件](https://www.zabbix.com/download_sources)，将里面mysql数据库里面的sql模板写入mysql的zabbix数据库。可能该模板不是以.gz为结尾的压缩文件，可以使用 gzip data.sql 压缩为 data.sql.gz，然后

   ```shell
   # 注意不能先写data.sql.gz，会报错不成功
   zcat ./schema.sql.gz | mysql -uzabbix -p zabbix
   zcat ./images.sql.gz | mysql -uzabbix -p zabbix
   zcat ./data.sql.gz | mysql -uzabbix -p zabbix
   ```

   正常通过apt install zabbix-server安装后，/usr/share/doc/zabbix-server-mysql 文件夹中会包含相应的模板文件的。

4. 设置开机自启 update-rc.d zabbix-server enable 时报错？

   解决方法：

   ```shell
   update-rc.d zabbix-server defaults 88
   update-rc.d zabbix-server enable
   ```

5. 安装 zabbix web 时出现问题，比如，php time_zone？

   解决方法：打开php.ini 或/etc/zabbix/apache.conf，设置 php_value date.timezone Asia/Shanghai

## 安装Zabbix-agent

方法如下，（注意：也是以root身份），假设安装zabbix-agent的服务器的IP=1.1.1.1

```shell
dpkg -i zabbix-release_5.0-1+bionic_all.deb
apt update
apt install zabbix-agent

# 先关闭zabbix-agent服务，因默认安装自启
service zabbix-agent stop

# vim /etc/zabbix/zabbix_agentd.conf
# 修改如下内容，注意1.1.1.0为zabbix-server所在服务器的 IP addr
Server=1.1.1.0
ServerActive=1.1.1.0
Hostname=1.1.1.1

# 然后重启zabbix-agent服务
service zabbix-agent start

# 查看日志文件，是否启动有错误
tail /var/log/zabbix-agent/zabbix_agentd.log
```

遇到的问题：

1. 安装出错？

   解决方法：同zabbix-server

2. 日志文件中出现hostname no found?

   解决方法：查看并修改 /etc/zabbix/zabbix_agentd.conf 中的 Hostname 为zabbix-agent所在的服务器的ip

3. 在zabbix web添加hosts时，显示的数据同zabbix-server所在机器的zabbix-agent一样？

   解决方法：这是在添加hosts时，设置Interfaces--> agent ip 时设置为127.0.0.1导致的，默认收集zabbix-server所在服务器的运行信息

## 其他问题

1. 为zabbix监控系统添加邮件报警，如果服务器可以连接互联网，可以选择使用 [常用邮件客户端软件设置](https://service.mail.qq.com/cgi-bin/help?id=28&no=371&subtype=1) ，来设置EMAIL
2. 监控--> Maps 中，可以自己编辑设置需要关注的服务器连接情况，可以添加删减，在两个服务器间连线（使用CTRL），查看两个服务器之间的带宽情况

