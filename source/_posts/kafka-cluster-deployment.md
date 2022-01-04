---
title: kafka集群部署
date: 2020-03-03 15:49:37
tags:
- kafka
- big data
categories: technology
---

Kafka最初是由领英(LinkedIn)开发，并随后于2011年初开源，并于2012年10月23日由Apache Incubator孵化出站。它是为处理实时数据提供一个统一、高吞吐、低延迟的[分布式](http://baike.baidu.com/view/402382.htm)发布订阅消息系统。Kafka由Java和Scala编写。根据2014年Quora的帖子，Jay Kreps似乎已经将它以作家[弗朗茨·卡夫卡](https://zh.wikipedia.org/wiki/弗朗茨·卡夫卡)命名。Kreps选择将该系统以一个作家命名是因为，它是“一个用于优化写作的系统”，而且他很喜欢卡夫卡的作品。Kafka架构的主要术语包括Topic、Record和Broker。Topic由Record组成，Record持有不同的信息，而Broker则负责复制消息。Kafka有四个主要API：

<!--more-->

- **生产者API**：支持应用程序发布Record流。
- **消费者API**：支持应用程序订阅Topic和处理Record流。
- **Stream API**：将输入流转换为输出流，并产生结果。
- **Connector API**：执行可重用的生产者和消费者API，可将Topic链接到现有应用程序。

下面进行Kafka集群部署工作。这里以3台服务器为例，假设IP分别是1.1.1.0， 1.1.1.1， 1.1.1.2，默认在1.1.1.0上演示。这里假设Java和Zookeeper已经部署成功，如果没有部署可以参考我之前的文章。

# 下载Kafka

从官网下载[Kafka](https://kafka.apache.org/downloads) ，这里下载2.4.0版本

```bash
wget http://mirrors.ocf.berkeley.edu/apache/kafka/2.4.0/kafka_2.11-2.4.0.tgz
```

# 安装Kafka

安装Kafka非常简单，直接解压即可

```bash
tar -xzf kafka_2.11-2.4.0.tgz
```

也可以指定解压目录，如Documents

```bash
tar -xzf kafka_2.11-2.4.0.tgz -C ./Documents/.
```

# 配置Kafka

进入Kafka安装目录

```bash
cd kafka_2.11-2.4.0/
```

创建日志文件夹，

```bash
mkdir log
vim config/server.properties
```

修改如下信息

```bash
broker.id=0
log.dirs=/home/jinzhongxu/kafka_2.11-2.4.0/log
zookeeper.connect=1.1.1.0:2181, 1.1.1.1:2181, 1.1.1.2:2181
```

把文件夹kafka_2.11-2.4.0/ 拷贝到另外两台服务器上

```bash
scp -r kafka_2.11-2.4.0/ 1.1.1.1:/home/jinzhongxu/.
scp -r kafka_2.11-2.4.0/ 1.1.1.2:/home/jinzhongxu/.
```

并分别修改（ `broker.id` 属性是集群中服务器的永久的固定的名字）

```bash
broker.id=1
```

和

```bash
broker.id=2
```

# 启动Kafka

进入Kafka安装目录

```
 ./bin/kafka-server-start.sh config/server.properties
```

运行命令

```bash
jps
```

查看Kafka是否启动成功。并在其他服务器上也分别启动Kafka。

# 测试Kafka

## 创建topic

```bash
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test
```

## 查看topic

```bash
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

## 创建producer

```bash
bin/kafka-console-producer.sh --broker-list localhost:909-topic test
```

## 创建consumer

```bash
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
```

# 删除Topic

首先，在config/server.properties中增加

```shell
delete.topic.enable=true
```

否则，并不能立刻删除该topic，而是标记为待删除(marked for deletion)，当重启Kafka时才会删除。

然后，查看该Kafka集群有哪些topic

```bash
./bin/kafka-topics.sh  --list --zookeeper localhost:2181 
```

最后，使用命令删除topic

```bash
./bin/kafka-topics.sh  --delete --zookeeper localhost:2181 --topic test
```

再次查看Kafka的topic发现已经没有了。