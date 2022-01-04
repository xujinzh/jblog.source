---
title: Cassandra 集群部署
date: 2020-02-28 16:27:15
tags:
- cassandra
- big data
categories: technology
---

Facebook参照Amazon的NoSQL的Dynamo和Google Big Table开发了Cassandra，它是一种NoSQL数据库。其cqlsh的运行依赖于python 2.x，**注意python3目前还不支持**。下面记载一下自己在6台服务器上搭建Cassandra集群的过程以及需要注意的点。这里以Ubuntu18.04为例。

<!--more-->

假设我们6个节点服务器的ip地址分别为1.1.1.0, 1.1.1.1, 1.1.1.2, 1.1.1.3, 1.1.1.4, 1.1.1.5，并希望把1.1.1.1， 1.1.1.2， 1.1.1.3 作为种子节点（一般不少于2个），其他为正常节点。下面操作默认在1.1.1.0机器上执行，并且默认各机器之间已经可以互相进行SSH免密连接。

# 下载Cassandra和JDK

Cassandra的下载地址是[Apache Cassandra](http://cassandra.apache.org/).

JDK目前推荐jdk8，下载地址是[Java SE Development Kit 8 Downloads](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html) .

# 安装Cassandra 和JDK



## 安装Cassandra

安装非常简单，将压缩包解压到你喜欢的目录即可

```bash
tar -xzf apache-cassandra-3.11.6-bin.tar.gz
```

## 安装JDK

安装也非常简单，只需要解压缩到喜欢的目录

```bash
tar -xzf jdk-8u241-linux-x64.tar.gz
```

# 配置Cassandra环境变量和Java环境变量

```bash
vim ~.bashrc
```

```shell
export JAVA_HOME=/home/jinzhongxu/jdk1.8.0_241
export CASSANDRA_HOME=/home/jinzhongxu/apache-cassandra-3.11.6
export PATH=$PATH:$JAVA_HOME/bin:$CASSANDRA_HOME/bin
```

然后，在主目录下，运行

```bash
sourc ~/.bashrc
```

```bash
java -version
```

没有报错，就说明配置成功。

# 配置Cassandra运行参数



进入配置文件目录

```bash
cd apache-cassandra-3.11.6/conf/
vim cassandra.yaml
```

首先，在1.1.1.0机器上修改

```shell
cluster_name: 'Cassandra Cluster' # 修改为自己喜欢的名字，后面运行后再修改比较麻烦
- seeds: "1.1.1.1,1.1.1.2,1.1.1.3"
listen_address: 1.1.1.0
rpc_address: 1.1.1.0
data_file_directories:
	- /var/lib/cassandra/data # 也可以添加多个用于存储数据
commitlog_directory: /var/lib/cassandra/commitlog
```

**注意：**

1. **seeds处ip与逗号之间最好不要有空格；**
2. **listen_address和rpc_address要使用IP地址，不要使用hostname;**
3. **此处需要给用户jinzhongxu添加访问上面目录的权限，**如下

```bash
sudo chown -R jinzhongxu /var/lib/cassandra/data
sudo chown -R jinzhongxu /var/lib/cassandra/commitlog
```

然后，将apache-cassandra-3.11.6拷贝到其他机器中

```bash
scp -r apache-cassandra-3.11.6 1.1.1.1:/home/jinzhongxu/.
```

并修改

```shell
listen_address: 1.1.1.1
rpc_address: 1.1.1.1
```

创建目录/var/lib/cassandra/data和/var/lib/cassandra/commitlog，并分配访问权限

其他机器1.1.1.2, 1.1.1.3, 1.1.1.4, 1.1.1.5, 同样执行，注意换成各自的ip

# 启动Cassandra集群服务

注意，需要先启动种子节点上的服务，然后才能启动正常节点上的服务。

先在1.1.1.1上运行

```bash
cassandra -f # 前台启动，在终端上打印日志
```

```bash
cassandra # 后台启动，不在终端打印日志
```

选择运行一种，建议第一种。

然后是1.1.1.2, 1.1.1.3

最后是1.1.1.4，1.1.1.5,  1.1.1.0

在机器1.1.1.1上运行

```bash
nodetool status
```

可以查看各节点运行情况是否正常

运行

```bash
cqlsh 1.1.1.0 9042
```

可以启动cql查询shell.

**如果不想带参数启动cqlsh，而是直接启动cqlsh**，可以通过如下方式设置：

1. 把配置文件cqlshrc.sample拷贝到~/.cassandra目录下（如果没有该目录，说明没有运行过Cassandra命令，可以运行一次命令：nodetool status，之后就会自动产生该目录）
2. 修改cqlshrc.sample为cqlshrc
3. 修改配置，将hostname=127.0.0.1切换为1.1.1.0，其他服务器一样改成各自IP地址
4. 重启Cassandra
5. 直接输入cqlsh，即启动本机cql shell
6. 输入cqlsh 1.1.1.1 9042 或 cqlsh 1.1.1.1，即远程连接服务器1.1.1.1的cql shell

# 关闭Cassandra

```bash
ps -aux | grep cassandra
```

kill 掉其相应的pid