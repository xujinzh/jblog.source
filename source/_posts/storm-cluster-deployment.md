---
title: storm 集群部署
author: Jinzhong Xu
date: 2020-03-08 13:19:08
tags:
- storm
- big data
categories: technology
---

Storm 是一个分布式计算框架，主要由Clojure编程语言编写，其主要编程语言是Java 和Clojure。最初是由Nathan Marz及其团队创建于BackType，该项目在被Twitter取得后开源。Storm集群部署需要Zookeeper和python的支持，假设这两个软件已经安装，并成功部署了Zookeeper集群。获取如何Standalone集群部署Zookeeper请参考我的文章：[Zookeeper集群部署](https://xujinzh.github.io/2020/03/03/zookeeper-cluster-deployment/) 

<!--more-->

下面分步骤进行Storm的集群部署，假设部署到三台服务器上，其IP地址分别是1.1.1.0， 1.1.1.1， 1.1.1.2，如果想部署更多台，可以后续随意增加，非常方便扩容，这是storm的一个优点。这里将1.1.1.0作为nimbus节点和UI节点，把1.1.1.1 和1.1.1.2 作为supervisor节点。假设部署的zookeeper集群分别是box0, box1, box2，其IP分别是1.1.10.0， 1.1.10.1， 1.1.10.2，并启动了zookeeper服务。

# 下载storm

从官网：[Apache Storm downloads](https://storm.apache.org/downloads.html) 下载喜欢的版本。这里以版本 [apache-storm-2.1.0.tar.gz](https://www.apache.org/dyn/closer.lua/storm/apache-storm-2.1.0/apache-storm-2.1.0.tar.gz) 为例进行安装部署。

# 安装Storm

解压缩下载的storm软件

```bash
tar -xzf apache-storm-2.1.0.tar.gz
mv apache-storm-2.1.0 storm
```

# 配置storm

```bash
cd storm/
mkdir data
vim ./conf/storm.yaml 
```

```shell
 storm.zookeeper.servers:
    - "1.1.10.0"
    - "1.1.10.1"
    - "1.1.10.2"
 nimbus.seeds: ["1.1.1.0"]
 storm.local.dir="/home/jinzhongxu/storm/data"
 supervisor.slots.ports:
  	- 6701
  	- 6702
  	- 6703
  	- 6704
```

**注意，这里需要严格保证空格，配置各项需要与侧边栏空一格，“-” 与配置参数空一格等**。

配置好后，将storm文件夹传到其他机器上。

```bash
scp -r storm 1.1.1.1:/home/jinzhongxu/.
scp -r storm 1.1.1.2:/home/jinzhongxu/.
```

# 启动storm

在机器1.1.1.0上，启动nimbus 和ui

```bash
cd storm
./bin/storm nimbus
./bin/storm ui
```

在机器1.1.1.1和1.1.1.2上分别启动supervisor

```bash
cd storm
./bin/storm supervisor
```

如果想要后台启动，可以运行如下类似命令

```bash
nohup ./bin/storm supervisor > /dev/null 2>&1 &
```

表示将日志文件（标准输出和标准错误）丢弃并在后台启动

# 查看storm集群启动情况

可以在各运行storm的集群上使用

```bash
jps
```

命令查看，如果发现有nimbus 或 supervisor则证明启动成功。

可以通过访问 1.1.1.0:8080 来查看storm UI

# 关闭storm

可以使用命令

```bash
jps
```

查看storm服务的pid，比如，pid=2384然后

```bash
kill 2384
```

杀死服务

# 关于storm的调优

1. 关于配置项 supervisor.slots.ports 表示该集群上为storm 的worker进程开通的端口号，一般一个worker需要消耗768+64=832M内存，当然，可以通过设置**worker.childopts: "-Xmx2048m"** 为2048M内存等。到底开几个端口需要看本机器的内存和cpu核心数。

2. 在定义一个拓扑时，可以通过 **conf.setNumWorkers()** 函数来指定一个 topolgoy 的 worker 数量，要小于（supervisor个数*每个supervisor的slots ports数）。如果worker数太大也不好，因为storm进程间通信比进程内耗费时间长，所以需要为topology设置一个合理的worker数。

3. 当storm与Kafka集成时，最好设置**Kafka Partition == Storm Spout**。

4. 当设置bolt 分组时，**优先使用localOrShuffleGrouping**代替shuffleGrouping，优先使用自带的分组而不是自己编写的分组方式。

5. 如果代码执行时间长，则需要通过增加Worker数量来将压力分散到更多的节点上以提升并发能力。**worker.heap.memory.mb、topology.worker.max.heap.size.mb**用来调整分配给每个 Worker的内存。当运行程序的Worker报出内存溢出的情况下，比较管用。

6. **topology.max.spout.pending:** 最大 Spout 挂起时间。一般Spout 的发射速度会快于下游的 Bolt 的消费速度，当下游的 Bolt 还有 pending中的 Tuple 没有消费完时，Spout 会停下来等待，该配置作用于 Spout 的每个 task。因此这个参数需要合理设置。conf.put(Config.TOPOLOGY_MAX_SPOUT_PENDING, 10000)。

7. **acker数量：**默认情况下，Storm 会在每个 worker 进程里面启动1个 acker 线程，以为 spout/bolt 提供 ack/fail 服务，该线程通常不太耗费资源，因此也无须配置过多，大多数情况下1个就足够了。最好 numAckers == numWorkers.

8. **storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10** 表示为拓扑mytopology 设置workers=5， blue-spout=3， yellow-bolt=10，其10个进程中包含3个spout和10个bolt并行运行拓扑。

9. 要找出并行度的最佳取值，主要结合 Storm UI 来做决策。

10. 操作系统配置，使用

    ```bash
ulimit -a
    ```

    查看

    1. open files：当前用户可以打开的文件描述符数；

    2. max user processes：当前用户可以运行的进程数，此参数太小将引起storm的一个错误，如下：

       ```shell
java.lang.OutOfMemoryError: unable to create new native thread
       	at java.lang.Thread.start0 (Native Method) [na:xxx]
	at java.lang.Thread.start (Thread.java: 640) [na:xxx]
       	at java.lang.UNIXProcess$1.run (UNIXProcess.java:141) ~ [na:xxx]
	at java.security.AccessController.doPrivileged (Native Method) ~ [na:xxx]
       ```

       
    
    

