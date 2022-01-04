---
title: TensorFlow 分布式简单介绍
mathjax: true
author: Jinzhong Xu
date: 2020-11-06 11:14:45
tags:
- dl
- tensorflow
categories:
- research
- machine learning
---

TensorFlow 能够方便的进行深度学习实践，特别是拥有多GPU的服务器或拥有多台GPU服务器时，如何使用 TensorFlow 进行快速训练，节约宝贵时间呢，下面介绍 TensorFlow 给出的分布式训练方法。

<!--more-->

# 单台服务器多GPU情况下

对于单台服务器下有多个GPU，TensorFlow 给出了镜像分布式策略 tensorflow.distribute.MirroredStrategy ，具体的使用是只需要实例化一个 MirroredStrategy 策略 （strategy = tensorflow.distribute.MirroredStrategy()），并把模型构建代码放置在 strategy.scope() 下就可以。

注意，在该策略下，可以指定参与计算的GPU，方法如下：

```python
# 指定计算的GPU为0,1
strategy = tensorflow.distribute.MirroredStrategy(devices=["/gpu:0", "/gpu:1"])
```

使用该策略进行模型 MobileNetV2  训练：

```python
# 安装 TensorFlow: pip install tensorflow
import tensorflow as tf

# 安装数据集工具: pip install tensorflow-datasets
import tensorflow_datasets as tfds

# 指定周期数和每个GPU上的批数据大小
num_epochs = 5
batch_size_per_replica = 64
learning_rate = 0.001

# 实例化镜像分布式策略
strategy = tf.distribute.MirroredStrategy()
print("Number of devices: %d" % strategy.num_replicas_in_sync)
# 模型总的批数量两为每个GPU的总和
batch_size = batch_size_per_replica * strategy.num_replicas_in_sync

# 预处理数据集
def resize(image, label):
    image = tf.image.resize(image, [224, 224]) / 255.0
    return image, label


# 载入数据集
dataset = tfds.load("cats_vs_dogs", split=tfds.Split.TRAIN, as_supervised=True)
dataset = dataset.map(resize).shuffle(1024).batch(batch_size)

# 把模型构建和编译放在镜像分布式策略下
with strategy.scope():
    model = tf.keras.applications.MobileNetV2(weights=None, classes=2)
    model.compile(
        optimizer=tf.keras.optimizers.Adam(learning_rate=learning_rate),
        loss=tf.keras.losses.sparse_categorical_crossentropy,
        metrics=[tf.keras.metrics.sparse_categorical_accuracy],
    )

# 进行模型训练
model.fit(dataset, epochs=num_epochs)
```

注意，MirroredStrategy 的步骤如下：

- 训练开始前，该策略在所有 N 个计算设备上均各复制一份完整的模型；
- 每次训练传入一个批次的数据时，将数据分成 N 份，分别传入 N 个计算设备（即数据并行）；
- N 个计算设备使用本地变量（镜像变量）分别计算自己所获得的部分数据的梯度；
- 使用分布式计算的 All-reduce 操作，在计算设备间高效交换梯度数据并进行求和，使得最终每个设备都有了所有设备的梯度之和；
- 使用梯度求和的结果更新本地变量（镜像变量）；
- 当所有设备均更新本地变量后，进行下一轮训练（即该并行策略是同步的）。

默认情况下，TensorFlow 中的 MirroredStrategy 策略使用 NVIDIA NCCL 进行 All-reduce (cross_device_ops=tf.distribute.HierarchicalCopyAllReduce())操作。除此之外，还有 tf.distribute.HierarchicalCopyAllReduce(), tf.distribute.ReductionToOneDevice(). devices=["/gpu:0", "/gpu:1"]，指定特定的GPU，默认是所有GPU同时使用。一种个性化策略如下：（更多参考 [TensorFlow 官网](https://www.tensorflow.org/guide/distributed_training))

```python
strategy = tf.distribute.MirroredStrategy(devices=["/gpu:0", "/gpu:1"], cross_device_ops=tf.distribute.HierarchicalCopyAllReduce)
```

 通常，当机器上GPU性能持平时，训练时间和GPU数量近似反比。

# 多台服务器情况下

对于多台服务器，TensorFlow 给出了 MultiWorkerMirroredStrategy，同时因为需要多服务器间通信，因此还需要其他的设置。主要是设置环境变量：TF_CONFIG，具体如下：

```python
os.environ["TF_CONFIG"] = json.dumps({
    "cluster": {
        "worker": ["host1:port", "host2:port", "host3:port"],
        "ps": ["host4:port", "host5:port"]
    },
   "task": {"type": "worker", "index": 1}
})
```

其中，cluster 表示服务器，里面包含 worker(用于计算梯度) 和 ps(Parameter Server，用于更新参数)，分别用IP地址和端口指定。task 指定运行的服务器，不同服务器上代码里的 task index 不同。其中 index 指明该服务器的角色，如上面试例代码中 task index = 1，表示 worker 里的 host2:port. 除此之外，多服务器还需要注意防火墙的配置，最好关闭防火墙。

```python
tf.distribute.experimental.MultiWorkerMirroredStrategy(
    communication=tf.distribute.experimental.CollectiveCommunication.AUTO,
    cluster_resolver=None
)
```

 communication 一共有三种方法可以选择，分别是 AUTO，RING 和 NCCL，默认是 AUTO.

实际运行时，服务器的代码都是一样的，除了TF_CONFIG 中的 task 配置，当在一台机器上运行代码后，它会进入监听状态，当所有服务器通信成功后，会自动进行训练。

```python
import json
import os

import tensorflow as tf
import tensorflow_datasets as tfds

num_epochs = 5
batch_size_per_replica = 64
learning_rate = 0.001

num_workers = 3
os.environ["TF_CONFIG"] = json.dumps(
    {
        "cluster": {
            "worker": ["localhost:20000", "localhost:20001"],
            "ps": ["localhost:20002"],
        },
        "task": {"type": "worker", "index": 0},
    }
)

strategy = tf.distribute.experimental.MultiWorkerMirroredStrategy()
batch_size = batch_size_per_replica * num_workers


def resize(image, label):
    image = tf.image.resize(image, [224, 224]) / 255.0
    return image, label


dataset = tfds.load("cats_vs_dogs", split=tfds.Split.TRAIN, as_supervised=True)
dataset = dataset.map(resize).shuffle(1024).batch(batch_size)

with strategy.scope():
    model = tf.keras.applications.MobileNetV2(weights=None, classes=2)
    model.compile(
        optimizer=tf.keras.optimizers.Adam(learning_rate=learning_rate),
        loss=tf.keras.losses.sparse_categorical_crossentropy,
        metrics=[tf.keras.metrics.sparse_categorical_accuracy],
    )

model.fit(dataset, epochs=num_epochs)
```

可以对比单服务器多GPU情况，代码只是稍微进行改动。训练效率也能达到上面多GPU的情况。

# 参考链接

1. [TensorFlow 分布式训练](https://tf.wiki/zh_hans/appendix/distributed.html)

2. [Distributed training with TensorFlow](https://tf.wiki/en/appendix/distributed.html)

3. [TensorFlow: Distributed training with TensorFlow](https://www.tensorflow.org/guide/distributed_training#multiworkermirroredstrategy)

4. [分布式TensorFlow入门教程](https://zhuanlan.zhihu.com/p/35083779)

   