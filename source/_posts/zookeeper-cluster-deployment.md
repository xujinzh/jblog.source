---
title: Zookeeper集群部署
date: 2020-03-03 14:53:49
tags:
- big data
- zookeeper
categories: technology
---

Zookeeper 来源于雅虎的一个研究小组，开发其用以提供分布式协调服务。**分布式应用程序可以基于 Zookeeper 实现诸如数据发布/订阅、负载均衡、命名服务、分布式协调/通知、集群管理、Master 选举、分布式锁和分布式队列等功能。**

<!--more-->

关于Zookeeper集群部署的一点建议：1、最好将Zookeeper部署在多于一台的集群上，即采用集群部署而不是单台服务器的standalone；2、最好部署在奇数(2N+1)台服务器上，因为Zookeeper通过多数大于少数来保证可用性，当N个节点不能访问时，整个Zookeeper集群仍然是可用的。

下面以3台服务器为例，来部署Zookeeper集群，假设它们的IP地址分别是1.1.1.0， 1.1.1.1， 1.1.1.2. 各步骤以1.1.1.0为默认机器演示。

# 下载Zookeeper

从官网下载[Zookeeper](http://mirror.bit.edu.cn/apache/zookeeper/) ，这里下载3.4.14版本

```bash
wget http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz
```

# 安装Zookeeper

直接解压缩

```bash
tar -xzf zookeeper-3.4.14.tar.gz
```

也可以选择解压缩到指定的目录，比如Documents

```bash
tar -xzf zookeeper-3.4.14.tar.gz -C ~/Documents/.
```

# 配置Zookeeper

配置zookeeper很关键，首先进入zookeeper配置文件夹

```bash
cd zookeeper-3.4.14/conf/
```

然后，利用模板创建配置文件

```bash
cp zoo_sample.cfg zoo.cfg
```

最好，修改配置参数，在此之前，我们需要创建文件夹用于存储数据和日志

```bash
cd zookeeper-3.4.14
mkdir data
mkdir log
echo 1 > /data/myid
```

```bash
vim zoo.cfg
```

```shell
dataDir=/home/jinzhongxu/zookeeper-3.4.14/data
dataLogDir=/home/jinzhongxu/zookeeper-3.4.14/log

server.1=0.0.0.0:2888:3888
server.2=1.1.1.1:2888:3888
server.3=1.1.1.2:2888:3888
```

将zookeeper文件夹拷贝到其他机器1.1.1.1和1.1.1.2上

```bash
scp -r zookeeper-3.4.14 1.1.1.1:/home/jinzhongxu/.
scp -r zookeeper-3.4.14 1.1.1.2:/home/jinzhongxu/.
```

并分别进入机器1.1.1.1中

```bash
echo 2 > zookeeper-3.4.14/data/myid
```

并修改配置

```bash
vim /conf/zoo.cfg
```

```shell
server.1=1.1.1.0:2888:3888
server.2=0.0.0.0:2888:3888
server.3=1.1.1.2:2888:3888
```

机器1.1.1.2中

```bash
echo 3 > zookeeper-3.4.14/data/myid
```

并修改配置

```bash
vim /conf/zoo.cfg
```

```shell
server.1=1.1.1.0:2888:3888
server.2=1.1.1.1:2888:3888
server.3=0.0.0.0:2888:3888
```

在上面的配置中，也可以将IP地址换成 /etc/hosts 映射的 hostname上，但是，当/etc/hosts中出现

```shell
127.0.1.1       box0

1.1.1.255     host
1.1.1.0     box0
1.1.1.1     box1
1.1.1.2     box2
```

此时，在 boxi 上必须使用

```
server.i=0.0.0.0:2888:3888
```

或者，注销掉 127.0.1.1，建议使用上面方法，不改变 /etc/hosts 里的 127.0.1.1       box0

```shell
#127.0.1.1       box0

1.1.1.255     host
1.1.1.0     box0
1.1.1.1     box1
1.1.1.2     box2
```

# 启动zookeeper

**分别在每个机器上**运行如下命令，**注意，分别启动完每个机器上的zkServer.sh后，再查看状态，因为zookeeper是选举机制，只启动1个就查看状态，一般是不成功的。**

```bash
cd zookeeper-3.4.14
/bin/zkServer.sh start
```

启动**所有机器上**的Zookeeper，查看zookeeper启动情况，运行如下命令

```bash
/bin/zkServer.sh status
```

运行成功结果显示这样：

```bash
ZooKeeper JMX enabled by default
Using config: /home/jinzhongxu/zookeeper/bin/../conf/zoo.cfg
Mode: leader
```

关闭Zookeeper，运行如下命令

```bash
/bin/zkServer.sh stop
```

# Zookeeper相关知识

- [Zookeeper集群部署](https://www.cnblogs.com/wxisme/p/5178211.html)
- [Zookeeper教程](https://www.w3cschool.cn/zookeeper/)

# 补充

- 从版本3.4.0开始，Zookeeper提供了自动清理快照和事务日志的功能，该配置在zoo.cfg里：

  1. autopurge.purgeInterval=1
  2. autopurge.snapRetainCount=3

  autopurge.purgeInterval：这个参数指定了持久化日志清理频率，单位是小时，默认是0，即不开启自动清理功能。

  autopurge.snapRetainCount：这个参数和上面的参数搭配，用于指定需要保留的持久化日志文件数目，默认是3个。