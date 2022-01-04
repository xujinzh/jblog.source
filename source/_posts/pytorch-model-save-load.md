---
title: PyTorch 模型保存和加载
mathjax: true
author: Jinzhong Xu
date: 2021-06-22 10:37:42
tags:
- dl
- python
- ml
- cv
categories:
- research
- deep learning
---

在训练深度学习模型时，需要定时保存最优模型；当模型训练时间过长，中断训练判断是否继续训练时，需要保存模型；当训练好的模型拿给别人推断时，需要保存模型，等等。模型的保存和加载在深度学习模型的研究和推理时经常使用。本文介绍如何在 PyTorch 中保存和加载模型。建议阅读文献3.

<!--more-->

# 基本方法

## torch.save()

将序列化对象保存到磁盘。此函数使用 Python 的 pickle 实用程序进行序列化。使用此功能可以保存各种对象的模型、张量和字典。

```python
torch.save(obj, f, pickle_module=<module 'pickle' from '/usr/local/miniconda/lib/python3.8/pickle.py'>, pickle_protocol=2)
```

```tex
obj – 保存对象
f － 类文件对象 (返回文件描述符)或一个保存文件名的字符串，一般保存文件的扩展名为 .pth 或者 .pt
pickle_module – 用于 pickling 元数据和对象的模块
pickle_protocol – 指定 pickle protocal 可以覆盖默认参数
```

```python
# 保存整个模型
torch.save(model,'best.pth')
# 保存模型参数（训练得到的权重）
torch.save(model.state_dict(), 'best.pth')
```



## torch.load()

从磁盘文件中读取一个通过`torch.save()`保存的对象。 `torch.load()` 可通过参数`map_location` 动态地进行内存重映射，使其能从不同设备中读取文件。一般调用时，需两个参数: storage 和 location tag. 返回不同地址中的 storage，或返回 None (此时地址可以通过默认方法进行解析)。如果这个参数是字典的话，意味着其是从文件的地址标记到当前系统的地址标记的映射。 默认情况下， location tags 中 "cpu" 对应 host tensors，‘cuda:device_id’ (e.g. ‘cuda:2’) 对应 cuda tensors。 用户可以通过 register_package 进行扩展，使用自己定义的标记和反序列化方法。 

```python
torch.load(f, map_location=None, pickle_module=<module 'pickle' from '/usr/local/miniconda/lib/python3.8/pickle.py'>)
```

```tex
f – 类文件对象 (返回文件描述符)或一个保存文件名的字符串
map_location – 一个函数或字典规定如何 remap 存储位置
pickle_module – 用于 unpickling 元数据和对象的模块 (必须匹配序列化文件时的 pickle_module )
```

```python
# 加载整个模型
torch.load('best.pth')
 
# 加载到 CPU
torch.load('best.pth', map_location=torch.device('cpu'))
 
# 使用函数加载到 CPU
torch.load('best.pth', map_location=lambda storage, loc: storage)
 
# 加载到 GPU 1
torch.load('best.pth', map_location=lambda storage, loc: storage.cuda(1))
 
# 从 GPU 1 加载到 GPU 0
torch.load('best.pth', map_location={'cuda:1':'cuda:0'})
 
# 通过 io.BytesIO 对象加载
with open('best.pth') as f:
    buffer = io.BytesIO(f.read())
```

## torch.nn.Module.load_state_dict(state_dict)

在 PyTorch 中，torch.nn.Module 模型的可学习参数（即权重和偏差）包含在模型的参数中（通过 model.parameters() 访问）。 state_dict 只是一个 Python 字典对象，它将每一层映射到其参数张量。请注意，只有具有可学习参数的层（卷积层、线性层等）和注册缓冲区（batchnorm 的 running_mean）在模型的 state_dict 中有条目。优化器对象 (torch.optim) 也有一个 state_dict，其中包含有关优化器状态的信息，以及使用的超参数。

由于 state_dict 对象是 Python 字典，因此它们可以轻松保存、更新、更改和恢复，从而为 PyTorch 模型和优化器添加了大量模块化。

```python
torch.nn.Module.load_state_dict(state_dict, strict=True)
```

```tex
state_dict - 保存 parameters 和 persistent buffers 的字典
strict - 可选，bool型。state_dict 中的 key 是否和 model.state_dict() 返回的 key 一致。
```

```python
import copy
import os
import random
import time

import matplotlib.pyplot as plt
import numpy as np
import torch
import torchvision

class MyModel(torch.nn.Module):
    def __init__(self):
        super(MyModel, self).__init__()
        self.fully_conv = torch.nn.Sequential(
            torch.nn.Conv2d(3, 96, kernel_size=(11, 11), stride=(2, 2), bias=True),
            torch.nn.BatchNorm2d(96),
            torch.nn.ReLU(),
            torch.nn.MaxPool2d(3, stride=2),
            torch.nn.Conv2d(
                96, 256, kernel_size=(5, 5), stride=(1, 1), groups=2, bias=True
            ),
            torch.nn.BatchNorm2d(256),
            torch.nn.ReLU(),
            torch.nn.MaxPool2d(3, stride=1),
            torch.nn.Conv2d(
                256, 384, kernel_size=(3, 3), stride=(1, 1), groups=1, bias=True
            ),
            torch.nn.BatchNorm2d(384),
            torch.nn.ReLU(),
            torch.nn.Conv2d(
                384, 384, kernel_size=(3, 3), stride=(1, 1), groups=2, bias=True
            ),
            torch.nn.BatchNorm2d(384),
            torch.nn.ReLU(),
            torch.nn.Conv2d(
                384, 32, kernel_size=(3, 3), stride=(1, 1), groups=2, bias=True
            ),
        )

    def forward(self, x):
        return self.fully_conv(x)
# 初始化模型   
model = MyModel()
# 初始化优化器
optimizer = torch.optim.SGD(model.parameters(), lr=0.001, momentum=0.9)
# 打印 model's state_dict
for param_tensor in model.state_dict():
    print(param_tensor.ljust(36, "."), model.state_dict()[param_tensor].size())
```

```python
fully_conv.0.weight................. torch.Size([96, 3, 11, 11])
fully_conv.0.bias................... torch.Size([96])
fully_conv.1.weight................. torch.Size([96])
fully_conv.1.bias................... torch.Size([96])
fully_conv.1.running_mean........... torch.Size([96])
fully_conv.1.running_var............ torch.Size([96])
fully_conv.1.num_batches_tracked.... torch.Size([])
fully_conv.4.weight................. torch.Size([256, 48, 5, 5])
fully_conv.4.bias................... torch.Size([256])
fully_conv.5.weight................. torch.Size([256])
fully_conv.5.bias................... torch.Size([256])
fully_conv.5.running_mean........... torch.Size([256])
fully_conv.5.running_var............ torch.Size([256])
fully_conv.5.num_batches_tracked.... torch.Size([])
fully_conv.8.weight................. torch.Size([384, 256, 3, 3])
fully_conv.8.bias................... torch.Size([384])
fully_conv.9.weight................. torch.Size([384])
fully_conv.9.bias................... torch.Size([384])
fully_conv.9.running_mean........... torch.Size([384])
fully_conv.9.running_var............ torch.Size([384])
fully_conv.9.num_batches_tracked.... torch.Size([])
fully_conv.11.weight................ torch.Size([384, 192, 3, 3])
fully_conv.11.bias.................. torch.Size([384])
fully_conv.12.weight................ torch.Size([384])
fully_conv.12.bias.................. torch.Size([384])
fully_conv.12.running_mean.......... torch.Size([384])
fully_conv.12.running_var........... torch.Size([384])
fully_conv.12.num_batches_tracked... torch.Size([])
fully_conv.14.weight................ torch.Size([32, 192, 3, 3])
fully_conv.14.bias.................. torch.Size([32])
```

```python
# 打印 optimizer's state_dict
for var_name in optimizer.state_dict():
    print(var_name.ljust(15, "."), optimizer.state_dict()[var_name])
```

```python
state.......... {}
param_groups... [{'lr': 0.001, 'momentum': 0.9, 'dampening': 0, 'weight_decay': 0, 'nesterov': False, 'params': [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17]}]
```

# 保存和加载模型进行推断(<font size=6 color=orange>推荐</font>)

```python
# 保存模型参数字典
torch.save(model.state_dict(), PATH)
# 加载模型
model = TheModelClass(*args, **kwargs)
model.load_state_dict(torch.load(PATH))
model.eval()
```

PyTorch 的 1.6 版本将 torch.save 切换为使用新的基于 zipfile 的文件格式。 torch.load 仍然保留了以旧格式加载文件的能力。如果出于任何原因您希望 torch.save 使用旧格式，请传递 kwarg _use_new_zipfile_serialization=False。

保存模型进行推理时，只需保存训练模型的学习参数即可。使用 torch.save() 函数保存模型的 state_dict 将为您以后恢复模型提供最大的灵活性，这就是为什么它是保存模型的推荐方法。

一个常见的 PyTorch 约定是使用 .pt 或 .pth 文件扩展名保存模型。

<font size=3 color=red>请记住，在运行推理之前，您必须调用 model.eval() 将 dropout 和批量归一化层设置为评估模式。不这样做会产生不一致的推理结果。</font>

请注意， load_state_dict() 函数采用字典对象，而不是保存对象的路径。这意味着在将保存的 state_dict 传递给 load_state_dict() 函数之前，您必须对其进行反序列化。例如，您不能使用 model.load_state_dict(PATH) 加载。

<font size=3 color=orange> 如果您只打算保留性能最佳的模型（根据获得的验证损失），请不要忘记 best_model_state = model.state_dict() 返回对状态的引用而不是其副本！您必须序列化 best_model_state 或使用 best_model_state = deepcopy(model.state_dict()) 否则您的最佳 best_model_state 将通过后续训练迭代不断更新。因此，最终的模型状态将是过拟合模型的状态。</font>

# 保存和加载整个模型

```python
# 保存模型
torch.save(model, PATH)
# Model class must be defined somewhere
model = torch.load(PATH)
model.eval()
```

这个保存/加载过程使用最直观的语法并且涉及最少的代码。以这种方式保存模型将使用 Python 的 pickle 模块保存整个模块。这种方法的缺点是序列化的数据绑定到特定的类和保存模型时使用的确切目录结构。这样做的原因是 pickle 不保存模型类本身。相反，它保存包含类的文件的路径，该路径在加载时使用。因此，您的代码在其他项目中使用或在重构后可能会以各种方式中断。

<font size=3 color=red>请记住，在运行推理之前，您必须调用 model.eval() 将 dropout 和批量归一化层设置为评估模式。不这样做会产生不一致的推理结果。</font>

# 保存和加载用于推理和/或恢复训练的通用检查点(Checkpoint)

```python
# 保存
torch.save({
            'epoch': epoch,
            'model_state_dict': model.state_dict(),
            'optimizer_state_dict': optimizer.state_dict(),
            'loss': loss,
            ...
            }, PATH)
# 加载            
model = TheModelClass(*args, **kwargs)
optimizer = TheOptimizerClass(*args, **kwargs)

checkpoint = torch.load(PATH)
model.load_state_dict(checkpoint['model_state_dict'])
optimizer.load_state_dict(checkpoint['optimizer_state_dict'])
epoch = checkpoint['epoch']
loss = checkpoint['loss']

model.eval()
# - or -
model.train()  
```

保存一般检查点以用于推理或恢复训练时，您必须保存的不仅仅是模型的 state_dict。保存优化器的 state_dict 也很重要，因为它包含在模型训练时更新的缓冲区和参数。您可能想要保存的其他项目是您离开的 epoch、最新记录的训练损失、外部 torch.nn.Embedding 层等。因此，这样的检查点通常比单独的模型大 2~3 倍.

要保存多个组件，请将它们组织在字典中并使用 torch.save() 序列化字典。一个常见的 PyTorch 约定是使用 .tar 文件扩展名保存这些检查点。

要加载项目，首先初始化模型和优化器，然后使用 torch.load() 在本地加载字典。从这里，您可以通过简单地按预期查询字典来轻松访问保存的项目。

<font size=3 color=red>请记住，在运行推理之前，您必须调用 model.eval() 将 dropout 和批量归一化层设置为评估模式。不这样做会产生不一致的推理结果。如果您希望恢复训练，请调用 model.train() 以确保这些层处于训练模式。</font>

# 保存多个模型为一个文件

```python
# 保存
torch.save({
            'modelA_state_dict': modelA.state_dict(),
            'modelB_state_dict': modelB.state_dict(),
            'optimizerA_state_dict': optimizerA.state_dict(),
            'optimizerB_state_dict': optimizerB.state_dict(),
            ...
            }, PATH)
# 加载
modelA = TheModelAClass(*args, **kwargs)
modelB = TheModelBClass(*args, **kwargs)
optimizerA = TheOptimizerAClass(*args, **kwargs)
optimizerB = TheOptimizerBClass(*args, **kwargs)

checkpoint = torch.load(PATH)
modelA.load_state_dict(checkpoint['modelA_state_dict'])
modelB.load_state_dict(checkpoint['modelB_state_dict'])
optimizerA.load_state_dict(checkpoint['optimizerA_state_dict'])
optimizerB.load_state_dict(checkpoint['optimizerB_state_dict'])

modelA.eval()
modelB.eval()
# - or -
modelA.train()
modelB.train()
```

保存由多个 torch.nn.Modules 组成的模型（例如 GAN、序列到序列模型或模型集合）时，您遵循与保存常规检查点时相同的方法。换句话说，保存每个模型的 state_dict 和相应优化器的字典。如前所述，您可以通过简单地将任何其他项目附加到字典中来保存可能有助于您恢复训练的任何其他项目。

一个常见的 PyTorch 约定是使用 .tar 文件扩展名保存这些检查点。

要加载模型，首先初始化模型和优化器，然后使用 torch.load() 在本地加载字典。从这里，您可以通过简单地按预期查询字典来轻松访问保存的项目。

<font size=3 color=red>请记住，在运行推理之前，您必须调用 model.eval() 将 dropout 和批量归一化层设置为评估模式。不这样做会产生不一致的推理结果。如果您希望恢复训练，请调用 model.train() 将这些层设置为训练模式。</font>

# 使用来自不同模型的参数的热启动模型

```python
# 保存A模型的参数
torch.save(modelA.state_dict(), PATH)
# 加班到B模型
modelB = TheModelBClass(*args, **kwargs)
modelB.load_state_dict(torch.load(PATH), strict=False)
```

部分加载模型或加载部分模型是迁移学习或训练新的复杂模型时的常见场景。利用经过训练的参数，即使只有少数可用，也将有助于热启动训练过程，并有望帮助您的模型比从头开始训练更快地收敛。

无论您是从缺少一些键的部分 state_dict 加载，还是加载比您加载的模型更多键的 state_dict，您都可以在 load_state_dict() 函数中将严格参数设置为 False 以忽略不匹配键。

如果要将参数从一层加载到另一层，但某些键不匹配，只需更改要加载的 state_dict 中参数键的名称，以匹配要加载到的模型中的键。

# 跨设备保存和加载模型

```python
# 保存模型
torch.save(model.state_dict(), PATH)
# 加载模型到CPU
device = torch.device('cpu')
model = TheModelClass(*args, **kwargs)
model.load_state_dict(torch.load(PATH, map_location=device))
```

在使用 GPU 训练的 CPU 上加载模型时，将 torch.device('cpu') 传递给 torch.load() 函数中的 map_location 参数。在这种情况下，张量底层的存储使用 map_location 参数动态重新映射到 CPU 设备。

# Save on GPU, Load on GPU

```python
# 保存
torch.save(model.state_dict(), PATH)
# 加载
device = torch.device("cuda")
model = TheModelClass(*args, **kwargs)
model.load_state_dict(torch.load(PATH))
model.to(device)
# Make sure to call input = input.to(device) on any input tensors that you feed to the model
```

在 GPU 上加载经过训练并保存在 GPU 上的模型时，只需使用 model.to(torch.device('cuda')) 将初始化模型转换为 CUDA 优化模型。此外，请务必在所有模型输入上使用 .to(torch.device('cuda')) 函数来为模型准备数据。请注意，调用 my_tensor.to(device) 会在 GPU 上返回 my_tensor 的新副本。它不会覆盖 my_tensor。因此，请记住手动覆盖张量：my_tensor = my_tensor.to(torch.device('cuda'))。

# Save on CPU, Load on GPU

```python
# 保存
torch.save(model.state_dict(), PATH)
# 加载
device = torch.device("cuda")
model = TheModelClass(*args, **kwargs)
model.load_state_dict(torch.load(PATH, map_location="cuda:0"))  # Choose whatever GPU device number you want
model.to(device)
# Make sure to call input = input.to(device) on any input tensors that you feed to the model
```

在 GPU 上加载经过训练并保存在 CPU 上的模型时，将 torch.load() 函数中的 map_location 参数设置为 cuda:device_id。这会将模型加载到给定的 GPU 设备。接下来，一定要调用 model.to(torch.device('cuda')) 将模型的参数张量转换为 CUDA 张量。最后，确保在所有模型输入上使用 .to(torch.device('cuda')) 函数来为 CUDA 优化模型准备数据。请注意，调用 my_tensor.to(device) 会在 GPU 上返回 my_tensor 的新副本。它不会覆盖 my_tensor。因此，请记住手动覆盖张量：my_tensor = my_tensor.to(torch.device('cuda'))。

# Saving `torch.nn.DataParallel` Models

```python
# 保存
torch.save(model.module.state_dict(), PATH)
# 加载
# Load to whatever device you want
```

torch.nn.DataParallel 是一个支持并行 GPU 使用的模型包装器。要一般地保存 DataParallel 模型，请保存 model.module.state_dict()。这样，您就可以灵活地以任何方式将模型加载到您想要的任何设备上。

# 参考链接

1. [SAVING AND LOADING MODELS](https://pytorch.org/tutorials/beginner/saving_loading_models.html)
2. [Pytorch：模型的保存与加载 torch.save()、torch.load()、torch.nn.Module.load_state_dict()](https://blog.csdn.net/weixin_40522801/article/details/106563354)
3. [Pytorch中模型的保存与迁移](https://zhuanlan.zhihu.com/p/419940503)

