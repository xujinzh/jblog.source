---
title: PyTorch 中 Dataset 和 DataLoader 类的使用方法
mathjax: true
author: Jinzhong Xu
date: 2021-10-12 18:58:21
tags:
- pytorch
- cv
- python
- vot
- dl
categories:
- research
- object tracking
---

近年来，在学术界和工业界基于 PyTorch 进行深度学习算法研究及模型部署越来越流行，甚至超过了 TensorFlow. 除了其基于动态图的特性外，最主要的是其语法更贴近 Python，容易开发实现和调试。本篇介绍 PyTorch 中为目标跟踪等视觉领域提供的两个基础类 Dataset 和 DataLoader，给出它们的使用方法。

<!--more-->

# 利用 PyTorch 进行深度学习训练的一般流程

1. 首先创建自定义的 Dataset 类 和 **Sampler** 类(数据采用策略)；
2. 创建自定义的 DataLoader 类；
3. DataLoader 依据 Dataset 和 Sampler 迭代产生训练数据提供给模型进行训练。

总的来说，DataLoader 负责批次调度数据，Sampler 负责数据调度的采样策略生成索引（默认整数），Dataset 负责通过索引提取数据。

Dataset 封装数据集，通过索引获取元素， Sampler 提供索引次序，DataLoader 是一个调度器，迭代 DataLoaderIter 的过程中，迭代Sampler 获得下一索引，并通过该索引使用 Fetcher（Fetcher 是对 Dataset 的封装，使得 DataLoader 代码与 Iterable-style/Map-style Dataset 解耦）获得对应元素。

# Dataset

Dataset 负责提供图像和标签索引。

Dataset 包含两类，分别是  Map 式数据集，Iterable 式数据集。Iterable 式数据集处理流式数据类，而 Map 式数据集处理常规数据类。目前 CV 中 Map 式数据集用的较多。

torch.utils.data.Dataset 类是所有数据集的抽象父类，如 torch.utils.data.IterableDataset 抽象类就继承自它。Iterable 式数据集都继承自 torch.utils.data.IterableDataset 抽象类， Map 式数据集都继承自 torch.utils.data.Dataset 抽象类。

## 内建 Dataset

PyTorch 提供了现成的 Dataset 子类，如果这些类不能满足个人实际业务需求，可以重写 torch.utils.data.Dataset 或 torch.utils.data.IterableDataset 抽象类，构建自定义子类。现成的子类有：

- Map 式 (继承自 torch.utils.data.Dataset)

1. TensorDataset : 每个样本通过沿第一维索引张量来检索
2. ConcatDataset : 此类可用于组装不同的现有数据集
3. Subset : 指定索引区间的数据集子集

- Iterable 式 (继承自 torch.utils.data.IterableDataset)

1. ChainDataset : 此类可用于组装不同的现有数据集流。这链接操作是即时完成的，因此连接大规模具有此类的数据集将是有效的

## 常使用的数据集

写好自定义 Dataset 类后，就可以使用。一般的，在 torchvision 中写好了一些 Dataset，我们可以直接下载常见的数据集并使用：

```python
from torchvision import datasets
dataset = datasets.MNIST('./images/mnist', download=True, train=True)
```

如果想要利用本地的图像数据集，可以如下：

```python
from torchvision import datasets
dataset = datasets.ImageFolder('./images/mnist', train=True)
```

包含的数据集有

```python
'CIFAR10', 'CIFAR100', 'EMNIST', 'FashionMNIST', 'QMNIST', 'MNIST', 'KMNIST', 'STL10', 'SVHN', 'PhotoTour', 'SEMEION', 'Omniglot', 'SBU', 'Flickr8k', 'Flickr30k', 'VOCSegmentation', 'VOCDetection', 'Cityscapes', 'ImageNet', 'Caltech101', 'Caltech256', 'CelebA', 'WIDERFace', 'SBDataset', 'VisionDataset', 'USPS', 'Kinetics400', 'HMDB51',  'UCF101', 'Places365', 'Kitti'
```

## 继承 torch.utils.data.Dataset 抽象类

对于上面内建的 Map 式 Dataset 不能满足业务需求的，可自定义 Dataset，即构建  torch.utils.data.Dataset 子类。

 Map 式数据集类表示从索引（key）到样本数据的映射。如：datasets[9] 表示第 9 个图像样本。

在编写 Map 式数据集类时需要继承 torch.utils.data.Dataset 抽象类，重写方法：

1. **\_\_getitem\_\_**(self, index) （必须重写）
2. **\_\_len\_\_**(self) (可选，建议重写)

通常，代码如下：

```python
from torch.utils import data
import numpy as np
from PIL import Image

class MyDataset(data.Dataset):
    def __init__(self):
        # TODO
        # 实例化文件路径或文件名列表等
        pass
    def __getitem__(self, index):
        # TODO
        # 1. 从文件中读取一个图片文件 (e.g. using numpy.fromfile, PIL.Image.open).
        # 2. 预处理图片文件 (e.g. torchvision.Transform).
        # 3. 返回图像数据对 (e.g. image and label).
        pass
    def __len__(self):
        # TODO
        # 返回整个图像数据集的大小，即图片的个数
        return 0
```

可以参考官方代码中的例子(以下代码在 Jupyter Notebook 中使用)：

```python
from torchvision import datasets

datasets.MNIST??
```

或者 

```python
class TensorDataset(Dataset[Tuple[Tensor, ...]]):
    r"""Dataset wrapping tensors.

    Each sample will be retrieved by indexing tensors along the first dimension.

    Args:
        *tensors (Tensor): tensors that have the same size of the first dimension.
    """
    tensors: Tuple[Tensor, ...]

    def __init__(self, *tensors: Tensor) -> None:
        assert all(tensors[0].size(0) == tensor.size(0) for tensor in tensors), "Size mismatch between tensors"
        self.tensors = tensors

    def __getitem__(self, index):
        return tuple(tensor[index] for tensor in self.tensors)

    def __len__(self):
        return self.tensors[0].size(0)
```



## 继承 torch.utils.data.IterableDataset 抽象类

Iterable 式数据集类表示在图像数据集上的一个可迭代的对象。适合处理流式数据，不适合随机存取。如：iter(datasets) 获取迭代器，然后使用 next 迭代实现遍历。

在编写 Iterable 式数据集类时需要继承 torch.utils.data.IterableDataset 抽象类，重写方法：

1. **\_\_iter\_\_**(self) （必须重写）

示例代码如下：

```python
import torch
import math

class MyIterableDataset(torch.utils.data.IterableDataset):
    def __init__(self, start, end):
        super(MyIterableDataset).__init__()
        assert end > start, "this example code only works with end >= start"
        self.start = start
        self.end = end

    def __iter__(self):
        worker_info = torch.utils.data.get_worker_info()
        if worker_info is None:  # single-process data loading, return the full iterator
            iter_start = self.start
            iter_end = self.end
        else:  # in a worker process
            # split workload
            per_worker = int(
                math.ceil((self.end - self.start) / float(worker_info.num_workers))
            )
            worker_id = worker_info.id
            iter_start = self.start + worker_id * per_worker
            iter_end = min(iter_start + per_worker, self.end)
        return iter(range(iter_start, iter_end))
```

可以参考官方代码中的例子(以下代码在 Jupyter Notebook 中使用)：

```python
from torch.utils.data import IterableDataset

IterableDataset??
```

# Sampler

Sampler 负责提供遍历数据集所有图像索引的方式。

PyTorch 实现了如下几类方式：

1. SequentialSampler
2. RandomSampler
3. SubsetRandomSampler
4. WeightedRandomSampler
5. BatchSampler
6. DistributedSampler

SequentialSampler 是顺序采样器。RandomSampler、SubsetRandomSampler、WeightedRandomSampler 是随机采样器。BatchSampler 是批次采样器，DistributedSampler 是分布式采样器。

如果内建采样器不能满足需求，可以自定义采样器，继承自 torch.utils.data.Sampler，需要重写方法：

1. **\_\_iter\_\_**(self) （必须重写）
2. **\_\_len\_\_**(self) (可选重写)

# DataLoader

Dataloader 结合数据集 Dataset 和采样器 Sampler，并提供可迭代的给定的数据集。Dataloader 负责加载数据，同时支持 Map 式和 Iterable 式 Dataset，支持单进程/多进程，还可以设置加载顺序(loading order)、批次大小(batch size)、CUDA固定内存(pin memory)等参数。在训练和测试深度学习网络中，我们直接遍历 Dataloader 来获取数据（data、label等），并将数据喂给网络用于前向传播。

常见的模型训练框架

```python
# 创建 Dateset 和 Sampler
dataset = MyDataset()
sampler = SequentialSampler()
# Dataset 传递给 DataLoader
dataloader = torch.utils.data.DataLoader(dataset=dataset, sampler=sampler, batch_size=64, shuffle=False, num_workers=8)
# DataLoader 迭代产生训练数据提供给模型
for i in range(epoch):
    for index, (img, label) in enumerate(dataloader):
        img, label = img.to(device), label.to(device).squeeze()
        opt.zero_grad()
        logits = model(img)
        loss = criterion(logits, label)
        pass
```

DataLoader 参数

```python
DataLoader(dataset, batch_size=1, shuffle=False, sampler=None,
           batch_sampler=None, num_workers=0, collate_fn=None,
           pin_memory=False, drop_last=False, timeout=0,
           worker_init_fn=None)
```

参数介绍： 

`dataset` (*Dataset*) – 定义好的 Map 式或者 Iterable 式数据集

`batch_size` (*python:int, optional*) 一个 batch 含有多少样本 (default: 1)

`shuffle` (*bool, optional*) – 每一个 epoch 的 batch 样本是相同还是随机 (default: False)。表示每一个 epoch 中训练样本的顺序是否相同，一般 True

`sampler` (*Sampler, optional*) – 决定数据集中采样的方法. 如果有，则 shuffle 参数必须为 False 

`batch_sampler` (*Sampler, optional*) 和 sampler 类似，但是一次返回的是一个 batch 内所有样本的 index。和 batch_size, shuffle, sampler, and drop_last 三个参数互斥

`num_workers` (*python:int, optional*) 多少个子程序同时工作来获取数据，多线程。 (default: 0)

`collate_fn` (*callable, optional*) 合并样本列表以形成小批量

`pin_memory` (*bool, optional*) 如果为 True，数据加载器在返回前将张量复制到 CUDA 固定内存中

`drop_last` (*bool, optional*) – 如果数据集大小不能被 batch_size 整除，设置为 True 可删除最后一个不完整的批处理。如果设为 False 并且数据集的大小不能被 batch_size 整除，则最后一个 batch 将更小。(default: False) 

`timeout` (*numeric, optional*) 如果是正数，表明等待从 worker 进程中收集一个 batch 等待的时间，若超出设定的时间还没有收集到，那就不收集这个内容了。这个 numeric 应总是大于等于0。 (default: 0) 

`worker_init_fn` (_callable, optional*) 每个 worker 初始化函数 (default: None)

# 参考链接

1. [一文读懂 PyTorch 中 Dataset 与 DataLoader](https://bbs.cvmart.net/articles/5467)
2. [PyTorch源码解析与实践（1）：数据加载Dataset，Sampler与DataLoader](https://zhuanlan.zhihu.com/p/270028097)

