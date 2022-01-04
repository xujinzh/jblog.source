---
title: CNN 模型计算力 FLOPs
mathjax: true
author: Jinzhong Xu
date: 2021-08-02 17:03:39
tags:
- cnn
- dl
- cv
- ml
- neural network
- machine learning
- pytorch
- python
categories:
- research
- deep learning
---

在撰写论文时，当进行模型比较时，常常会比较模型准确度，但模型计算力消耗、参数个数也是重要的指标，特别是模型落地时。

<!--more-->

# FLOPs

FLOPS 与 FLOPs 不同:

- FLOPS: FLoating point Operations Per Second，表示每秒浮点运算次数，衡量硬件（如 GPU ）的计算性能，可理解为计算速度；
- FLOPs: FLoating point Operations，表示浮点运算数，衡量模型（如深度学习模型 CNN ）的复杂程度，可理解为计算量。 

# 计算 FLOPs

计算 FLOPs 常常指深度学习模型前向传播是的计算量，而 CNN 中计算量主要集中在卷积层，除此之外，也有池化层、批归一化层、激活层，上采样层等。下面主要介绍卷积层计算 FLOPs 的方法。

假设卷积核的大小是 $k \times k$，通道数是 $c$，输出特征图的大小是 $H \times W$​​，通道数是 $C$​，则计算力消耗是

$k \times k \times c \times H \times W \times C$

# 开源库

一种方便的开源库是 [ptflops](https://github.com/sovrasov/flops-counter.pytorch)

```bash
pip install ptflops
```

```python
import torchvision.models as models
import torch
from ptflops import get_model_complexity_info

with torch.cuda.device(0):
  net = models.densenet161()
  macs, params = get_model_complexity_info(net, (3, 224, 224), as_strings=True,
                                           print_per_layer_stat=True, verbose=True)
  print('{:<30}  {:<8}'.format('Computational complexity: ', macs))
  print('{:<30}  {:<8}'.format('Number of parameters: ', params))
```

# 参考链接

1. [CNN 模型所需的计算力flops是什么？怎么计算？](https://zhuanlan.zhihu.com/p/137719986)
2. [FLOPS ？FLOPs！！ FLOPs ? MACs！！](https://zhuanlan.zhihu.com/p/364543528)

