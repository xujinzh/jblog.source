---
title: zookeeper 和 storm 安装与启动
date: 2020-01-16 10:37:52
tags:
- zookeeper
- storm
- big data
categories: technology
---

Storm的运行需要Zookeeper的协助，主要是为了协调Storm无状态的Master守护进程Nimbus和Worker守护进程Supervisor。所以启动Storm之前需要首先启动Zookeeper，下面分三步进行介绍，注意，这里以单机模式测试，关于Zookeeper的Standalone集群部署请参考我的另一篇文章：[Zookeeper集群部署](https://xujinzh.github.io/2020/03/03/zookeeper-cluster-deployment/), 关于storm的Standalone集群部署请参考我的文章：[storm集群部署](https://xujinzh.github.io/2020/03/08/storm-cluster-deployment/) 

<!--more-->

# 安装jdk

首先从Oracle官网[下载jdk](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

其次，解压该压缩包

```bash
tar -xzf jdk*
```

最后，将Java路径添加到环境变量

# 安装Zookeeper和Storm

首先从官网[下载Zookeeper](http://zookeeper.apache.org/releases.html#download) (**注意：3.4.14版本测试通过，而3.5.6不成功**)和[下载Storm](https://storm.apache.org/)

其次，解压两个压缩包

```bash
tar -xzf zookeeper-3.4.14.tar.gz
tar -xzf apache-storm-2.1.0.tar.gz
```

最后，配置Zookeeper和Storm

1. 配置Zookeeper

```bash
cd zookeeper
mkdir data
cd conf/
cp zoo_sample.cfg zoo.cfg
vim zoo.cfg
```

修改

```shell
dataDir=/home/jinzhongxu/zookeeper/data
```



2. 配置Storm

```bash
cd storm
mkdir data
cd conf/
cp storm.yaml storm.yaml.bak
vim storm.yaml
```

修改（**注意：‘-’ 与后面的参数值之间有一个空格**）

```shell
storm.zookeeper.servers:
    - "localhost"
storm.local.dir: "/home/jinzhongxu/storm/data"
nimbus.host: "localhost"
supervisor.slots.ports:
    - 6700
    - 6701
    - 6702
    - 6703
```

# 启动zookeeper和storm

## 启动zookeeper

```bash
cd zookeeper
bin/zkServer.sh start
```



## 启动storm

```bash
cd storm
bin/storm nimbus
bin/storm supervisor
bin/storm ui
```

也可以使用如下方式，以后台方式打开

```bash
cd storm
nohup ./bin/storm nimbus > /dev/null 2>&1 &
nohup ./bin/storm supervisor > /dev/null 2>&1 &
nohup ./bin/storm ui > /dev/null 2>&1 &
```

1. nohup: 表示no hang up，即一直运行
2. \> /dev/null: 表示将日志输出丢弃到/dev/null中，即直接丢弃，不存储到服务器上
3. 2\>&1: 表示将2的错误输出（stderr）和1的标准输出（stdout）只打开一个通道一起写入1即/dev/null中
4. 最后一个&: 表示以后台方式运行，而不在终端上打印日志信息

打开本地浏览器，输入网址 http://localhost:8080

# 关闭

可以使用命令jps显示是否运行成功，比如单台机器情况如下：

```bash
jinzhongxu@jinzhongxu-PowerEdge-R740:~$ jps
34530 Jps
11305 Main
25930 QuorumPeerMain
33789 UIServer
33373 Supervisor
32910 Nimbus
```

如果想关闭进行，可直接kill掉相应的pid

```bash
kill 32910
```

