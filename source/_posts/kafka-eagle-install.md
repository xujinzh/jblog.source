---
title: Kafka 监控软件 Kafka-Eagle 安装教程
author: Jinzhong Xu
mathjax: true
date: 2020-07-17 17:00:25
tags:
- kafka
- kafka-eagle
categories: technology
---

Kafka-Eagle 是一款国产Kafka监控软件，总体上还是挺不错的。这里介绍如何安装它。

## 下载 Kafka-Eagle

从官网下载，地址为：http://download.kafka-eagle.org/

选择 Direct File Download

<!--more-->

## 安装 Kafka-Eagle

可以参考官网教程进行安装，教程地址，https://www.kafka-eagle.org/articles/docs/documentation.html，选择对应的系统类型。

这里给出自己安装的步骤以及需要注意的点。

```shell
tar -xzf kafka-eagle-bin-2.0.0.tar.gz
cd kafka-eagle-bin-2.0.0
tar -xzf kafka-eagle-web-2.0.0-bin.tar.gz
mv kafka-eagle-web-2.0.0 ../kafka-eagle-web
```

这里很奇怪，为什么解压后还要有一个压缩，搞不懂！

记录下，kafka-eagle-web的目录，我这里是

```shell
/home/jinzhongxu/kafka-eagle-web/
```

配置 kafka-eagle 环境，类似于官网

```shell
sudo vim /etc/profile
export JAVA_HOME=/home/jinzhongxu/jdk-11.0.7
export KE_HOME=/home/jinzhongxu/kafka-eagle-web
export PATH=$PATH:$KE_HOME/bin:$JAVA_HOME/bin
```

使之生效方法

```bash
. /etc/profile
# 或者
source /etc/profile
```

修改配置

**这里注意，1.5版本后，不在提供ke.sql。默认/home/jinzhongxu/kafka-eagle-web/db/ 是空文件夹，运行后，里面会自动出现ke.db**

```shell
vim kafka-eagle-web/conf/system-config.properties

# 改成自己的zookeeper节点
kafka.eagle.zk.cluster.alias=cluster
cluster.zk.list=box0:2181,box1:2181:box2:2181
#cluster1.zk.list=tdn1:2181,tdn2:2181,tdn3:2181
#cluster2.zk.list=xdn10:2181,xdn11:2181,xdn12:2181

# 改成自己的 kafka.eagle.url
######################################
# kafka sqlite jdbc driver address
######################################
kafka.eagle.driver=org.sqlite.JDBC
kafka.eagle.url=jdbc:sqlite:/home/jinzhongxu/kafka-eagle-web/db/ke.db
kafka.eagle.username=root
kafka.eagle.password=www.kafka-eagle.org
```

## 运行 Kafka-Eagle

```shell
./ke.sh start
```

这是会提示打开地址：http://127.0.1.1:8048    

（当然，也可以直接输入你的hostname，我这里是box0），输入用户名：admin，输入密码：123456

```shell
# 运行zookeeper/kafka来查看监控数据

zookeeper-3.5.8/bin/zkServer.sh start
nohup kafka_2.11-2.4.1/bin/kafka-server-start.sh kafka_2.11-2.4.1/config/server.properties > /dev/null 2>&1 &
```

# kafka 监控显示问题

搭建完后，总是不能显示监控的 kafka 数据，只能显示部分数据，这很可能是因为 kafka jmx 配置问题，解决方法如下：

1. 首先保证 kafka eagle 的配置文件 system-config.properties 中的

   ```bash
   kafka.eagle.metrics.charts=true
   ```

   

2. 打开 kafka broker 的 jmx 端口

   ```bash
   vim kafka-server-start.sh
   # 配置 JMX 端口，可随意指定
   if [ "x$KAFKA_HEAP_OPTS" = "x" ]; then
       export KAFKA_HEAP_OPTS="-server -Xms2G -Xmx2G -XX:PermSize=128m -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -XX:ParallelGCThreads=8 -XX:ConcGCThreads=5 -XX:InitiatingHeapOccupancyPercent=70"
       export JMX_PORT="9999"
   fi
   # 配置 RMI 
   exec $base_dir/kafka-run-class.sh $EXTRA_ARGS -Djava.rmi.server.hostname=该kafka broker的ip   kafka.kafka "$@"
   ```

   

3. 重启 kafka 和 kafka eagle

   ```bash
   # 重启 kafka
   ./kafka-server-start.sh ../config/server.properties
   
   # 重启 kafka eagle
   ./ke.sh restart
   ```

   ## 参考链接
   
   [kafka jmx](https://www.kafka-eagle.org/articles/docs/quickstart/metrics.html)
   
   [kafka jmx telnet](https://www.kafka-eagle.org/articles/docs/architecture/collect.html)
   
   [kafka eagle Install on Linux/macOS](https://www.kafka-eagle.org/articles/docs/installation/linux-macos.html)
   
   [kafka实践三：监控利器kafka-eagle](https://blog.csdn.net/yezonggang/article/details/97749786)
   
   [关于JMX三个端口的说明](https://blog.csdn.net/caidongxuan/article/details/105247609)
   
   [kafka-eagle 使用配置及远程jmx端口设置遇到的问题](https://blog.csdn.net/wk_dream/article/details/108086266)