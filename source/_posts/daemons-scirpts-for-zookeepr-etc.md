---
title: 一个脚本启动、关闭和查看大数据处理平台集群Zookeeper、Kafka等
author: Jinzhong Xu
mathjax: true
date: 2020-03-28 16:48:02
tags:
- shell
- big data
categories: technology
---

对于分布式大数据处理框架，如Zookeeper、Kafka、Storm等，在启动、关闭、查看状态时都需要一个一个的去各节点机器上查看，效率非常低，这里给出一个脚本，用于管理大数据处理框架如Zookeeper集群，具体脚本代码参看我的GitHub网站：https://github.com/xujinzh/archive/tree/master/big-data	

<!--more-->

命令使用说明，这里假设Zookeeper集群部署在box0, box1, box2上，我们在本地可以通过ssh连接这些集群服务器。在本地运行shell脚本。注意，这里的脚本需要你修改为自己的启动参数，如用户名，启动初始位置等。可以通过vim zks-daemons.sh来修改相应内容。

```bash
(base) jinzhongxu@jinzhongxu-PowerEdge-R740:~/Documents/daemons$ ./zks-daemons.sh status
ZooKeeper JMX enabled by default
Using config: /home/jinzhongxu/zookeeper/bin/../conf/zoo.cfg
Mode: leader
Zookeeper node is box0, run status command.
ZooKeeper JMX enabled by default
Using config: /home/jinzhongxu/zookeeper/bin/../conf/zoo.cfg
Mode: follower
Zookeeper node is box1, run status command.
ZooKeeper JMX enabled by default
Using config: /home/jinzhongxu/zookeeper/bin/../conf/zoo.cfg
Mode: follower
Zookeeper node is box2, run status command.
```

可以发现，box0被选为leader，box1和box2为follower.

同样的，我们可以关闭zookeeper集群

```bash
 ./zks-daemons.sh stop
```

启动Zookeeper集群

```bash
 ./zks-daemons.sh start
```

类似的，我们可以使用脚本管理Kafka集群等。