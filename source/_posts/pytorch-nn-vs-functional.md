---
title: PyTorch 中的 nn 和 nn.functional
mathjax: true
author: Jinzhong Xu
date: 2021-06-21 13:28:26
tags:
- python
- dl
- ml
categories:
- research
- deep learning
---

PyTorch 中有两个模块 torch.nn 和 torch.nn.functional 都可以用来构造深度学习神经网络模型，实现相同的功能，然而它们之间有着重要的差别。

<!--more-->

大体上来说，torch.nn 更抽象，是对 torch.nn.functional 更高一层的实现，有点 keras vs. tensorflow（1.x） 的感觉。torch.nn 中的，如 ReLU、Conv2d等，是类，而 torch.nn.functional 中的，如 relu、conv2d 等，是函数。

# torch.nn

torch.nn 里面的类是 torch.nn.functional 中函数的封装，两者都继承于 torch.nn.Module，因此 torch.nn 除了具有 torch.nn.functional 的功能外，还附带了 torch.nn.Module 相关的属性和方法，如 train(), eval(), load_state_dict, state_dict 等。

<font size=3 color=red> torch.nn 需要先实例化，然后对实例化对象传入数据：</font>

```python
import torch

inputs = torch.rand(64, 3, 217, 217)
# 实例化
conv = torch.nn.Conv2d(in_channels=3, out_channels=64, kernel_size=3, padding=1)
# 实例化对象处理数据
out = conv(inputs)
```

<font size=3 color=red>torch.nn 可以与 torch.nn.Sequential 结合</font>

```python
import torch

neural_network = torch.nn.Sequential(
    torch.nn.Conv2d(in_channels=3, out_channels=64, padding=1),
    torch.nn.BatchNorm2d(num_features=64),
    torch.nn.ReLU(),
    torch.nn.MaxPool2d(kernel_size=2),
    torch.nn.Dropout(0.5),
    torch.nn.Linear(in_features=32, out_features=10),
)
```



# torch.nn.functional

torch.nn.functional 中定义的是函数接口，其同样继承自 torch.nn.Module.

<font size=3 color=red>torch.nn.functional 需要传入数据和参数（如 weights, bias）</font>

```python
import torch

weights = torch.rand(64, 3, 3, 3)
bias = torch.rand(64)
inputs = torch.rand(64, 3, 217, 217)
# 调用函数
out = torch.nn.functional.conv2d(input=inputs, weight=weights, bias=bias, padding=1)
```

<font size=3 color=red>torch.nn 中的类是通过 torch.nn.functional 定义的</font>

```python
import torch.nn.functional as F
class ReLU(Module):
    r"""Applies the rectified linear unit function element-wise:

    :math:`\text{ReLU}(x) = (x)^+ = \max(0, x)`

    Args:
        inplace: can optionally do the operation in-place. Default: ``False``

    Shape:
        - Input: :math:`(N, *)` where `*` means, any number of additional
          dimensions
        - Output: :math:`(N, *)`, same shape as the input

    .. image:: ../scripts/activation_images/ReLU.png

    Examples::

        >>> m = nn.ReLU()
        >>> input = torch.randn(2)
        >>> output = m(input)


      An implementation of CReLU - https://arxiv.org/abs/1603.05201

        >>> m = nn.ReLU()
        >>> input = torch.randn(2).unsqueeze(0)
        >>> output = torch.cat((m(input),m(-input)))
    """
    __constants__ = ['inplace']
    inplace: bool

    def __init__(self, inplace: bool = False):
        super(ReLU, self).__init__()
        self.inplace = inplace

    def forward(self, input: Tensor) -> Tensor:
        return F.relu(input, inplace=self.inplace)

    def extra_repr(self) -> str:
        inplace_str = 'inplace=True' if self.inplace else ''
        return inplace_str
```

注意：<font size=3 color=cyan>in-place</font> operation 在 pytorch 中是指改变一个 tensor 的值的时候，不经过复制操作，而是直接在原来的内存上改变它的值。可以把它称为原地操作符。

在 pytorch 中经常加后缀 `_` 来代表原地 in-place operation，比如说 .add\_() 或者.scatter\_(). 我们可以将 in_place 操作简单的理解类似于python 中的 +=, -= 等操作。

对于 requires_grad=True 的 叶子张量 (leaf tensor) 不能使用 inplace operation; 对于在 求梯度阶段需要用到的张量 不能使用 inplace operation.

<font size=3 color=cyan>torch 中设置随机数种子，保证结果复现：</font>

```python
import random

import numpy as np
import torch


def setup_seed(seed):
    torch.manual_seed(seed)
    torch.cuda.manual_seed_all(seed)
    np.random.seed(seed)
    random.seed(seed)
    torch.backends.cudnn.deterministic = True
```



# 总结

1. 具有学习参数的 （如 Conv2d, BatchNorm2d, Linear）采用 torch.nn 的方式，没有学习参数的（如 MaxPool2d, loss func, activation func）等根据个人选择使用 torch.nn.functional 或者 torch.nn 的方式。

2. 当模型较复杂时，推荐 torch.nn，当其不能满足功能需求时，torch.nn.functional 是最佳选择，如自定义模块时。因为其更接近底层，更加灵活。

3. 在前馈网络计算（forward）时，torch.nn.functional 中的参数无法保存，即没有状态参数。状态参数一般指的是可学习的参数，如卷积层的 weights 和 bias，或根据需要自定义的一些可学习参数。torch.nn.functional 中涉及的参数一般是超参，无法利用反向传播进行学习。

   ```python
   import torch
   # 保存最好的模型参数
   torch.save(model,'best.pth')
   # 再次训练时
   model.load_state_dict(torch.load("best.pt"))  # model.load_state_dict()函数把加载的权重复制到模型的权重中去
   ```

   

4. <font size=3 color=cyan>关于 dropout，推荐使用 torch.nn 的方式</font>，因为一般情况下只有训练阶段才进行 dropout，在 eval 阶段都不会进行 dropout， 使用 torch.nn 方式定义 dropout，在调用 model.eval() 之后，model 中所有的 dropout layer 都关闭，但以 torch.nn.function.dropout 的方式定义 dropout，在调用 model.eval() 之后并不能关闭 dropout. 

# 参考链接

1. [PyTorch之nn.ReLU与F.ReLU的区别](https://blog.csdn.net/u011501388/article/details/86602275)
2. [Converting F.relu() to nn.ReLU() in PyTorch](https://www.joeltok.com/blog/pytorch-convert-f-relu-to-nn-relu)
3. [PyTorch之nn.ReLU与F.ReLU的区别介绍](https://cloud.tencent.com/developer/article/1734426)
4. [PyTorch 中，nn 与 nn.functional 有什么区别？](https://www.zhihu.com/question/66782101/answer/579393790)

