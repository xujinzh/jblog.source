---
title: 如何查看服务器上的 GPU 信息
mathjax: true
author: Jinzhong Xu
date: 2021-05-26 10:30:54
tags:
- gpu
- server
- nvidia-smi
categories:
- technology
- ubuntu
---

服务器上安装好了 GPU，同时也安装好了 nvidia drivers、cuda-toolkit、cudnn，那么如何查看 GPU 是否可以正常运行，已经如何读懂各参数信息呢？下面以 Ubuntu 18.04 来说明。

<!--more-->

# nvcc

cuda 编译器驱动程序 nvcc 的目的是向开发人员隐藏 cuda 编译的复杂细节。详细介绍请参考[英伟达官网](https://docs.nvidia.com/cuda/cuda-compiler-driver-nvcc/index.html)

这里主要是使用该程序查看 cuda 编译器工具的版本

```shell
# 查看 nvcc 路径
$ whereis nvcc
nvcc: /usr/local/cuda-10.0/bin/nvcc.profile /usr/local/cuda-10.0/bin/nvcc /usr/share/man/man1/nvcc.1
# 查看 cuda 编译器工具的版本信息
$ nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Sat_Aug_25_21:08:01_CDT_2018
Cuda compilation tools, release 10.0, V10.0.130
# 查看 cuda 编译器工具的版本信息
-version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2018 NVIDIA Corporation
Built on Sat_Aug_25_21:08:01_CDT_2018
Cuda compilation tools, release 10.0, V10.0.130
```

可以看到安装的 cuda 版本是 10.0.130

# nvidia-smi

nvidia-smi (NVIDIA System Management Interface) 是一种命令行实用工具，帮助管理和监控 NVIDIA GPU 设备。

```shell
$ nvidia-smi
Wed May 26 11:14:18 2021
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 460.73.01    Driver Version: 460.73.01    CUDA Version: 11.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  Tesla V100-PCIE...  Off  | 00000000:3B:00.0 Off |                    0 |
| N/A   22C    P0    33W / 250W |      0MiB / 32510MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   1  GeForce GTX TIT...  Off  | 00000000:AF:00.0 Off |                  N/A |
| 20%   29C    P0    70W / 250W |      0MiB / 12212MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   2  GeForce GTX TIT...  Off  | 00000000:D8:00.0 Off |                  N/A |
| 20%   26C    P0    64W / 250W |      0MiB / 12212MiB |      2%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+

```

显示参数解释：

1. **Wed May 26 11:14:18 2021** ：表示命令运行时间，也是指代  GPU 运行情况的具体时间
2. **NVIDIA-SMI 460.73.01    Driver Version: 460.73.01    CUDA Version: 11.2** ：表示安装的 cuda 驱动器版本和 cuda 版本
3. **GPU** ：表示 GPU 编号，从0开始
4. **Fan** ：表示 GPU 风扇转速，0-100%，当为 N/A 时表示设备不依赖风扇降温
5. **Name** ：表示 GPU 名称
6. **Temp** ：表示 GPU 当前运行温度
7. **Perf** ：表示 GPU 性能状态，P0 ~ P12，P0 表示性能最大（一般 GPU 未使用状态性能，当使用时性能降低），P12 表示性能最小
8. **Persistence-M** ：表示持续模式的状态，默认关闭。持续模式打开，当新的 GPU 应用启动时，花费时间更少。但持续模式打开耗能大
9. **Pwr:Usage/Cap** ：表示 GPU 能耗情况，包括当前消耗功率和最大功率
10. **Bus-Id** ：表示 GPU 总线信息
11. **Disp.A** ：表示 Display Active 的意思，表示GPU的显示是否初始化
12. **Memory-Usage** ：表示 GPU 显存情况，包括当前消耗显存以及最大显存
13. **GPU-Util** ：表示 GPU 的利用率，当 GPU 没有使用时，显示 0%，当 GPU 使用时，显示大于 0%
14. **Volatile Uncorr. ECC** ：ECC (error correcting code, 错误检查和纠正) 该功能可以提高数据的正确性，随之而来的是可用显存的减少和性能上的损失。
15. **Compute M.** ：表示 GPU 计算模式
16. **MIG M.** ： Multi-Instance GPU （MIG） 模式，创建基于 MIG 的 vGPU 实例需要将 GPU 切换到 MIG 模式，此配置为持久配置，只需执行一次，重启服务器后仍然有效: nvidia-smi -mig 1
17. **Processes** ：表示使用 GPU 的进程信息

## cuda 版本号不一致？

细心的同志会发现使用 `nvcc -V` 和使用 `nvidia-smi` 查看的 cuda 版本号不一致，这是什么原因呢。

nvcc 是 cuda 编译器，nvidia-smi 是NVIDIA System Management Interface，一种命令行工具。`nvcc -V` 显示的 cuda 版本对应 runtime api (运行时)，`nvidia-smi` 对应 driver api；nvcc 是通过 cuda-toolkit 安装，`nvidia-smi` 是通过 nvidia drivers 安装；nvcc 只知道自身构建是的 cuda runtime 版本，不知道安装的 nvidia drivers 版本，甚至不知道服务器上是否安装了 GPU driver.

通常，driver api 的版本能向下兼容 runtime api 的版本。因此，当 `nvidia-smi` 显示的 cuda 版本大于 `nvcc -V` 显示的 cuda 版本时没有问题，但是反过来小于就不行。

## pytorch 安装基于哪个 cuda 版本？

在 pytorch 官网上可以发现，当使用 conda 安装 pytorch 时，指定的版本信息是 cudatoolkit，如下

```shell
conda install pytorch torchvision torchaudio cudatoolkit=10.2 -c pytorch
```

因此，我们应该选择与 `nvcc -V` 对应的 cuda 版本来安装 pytorch. 另外，只要大版本对应即可，如 cudatooklit=10.x 对应 Cuda compilation tools, release 10.0, V10.0.130，完全可以使用。

# 其他 nvidia-smi 参数

```shell
# 实时刷新显示
$ nvidia-smi -l
# 每5s刷新一次
$ watch -n 5 nvidia-smi
# 列出所有 GPU 名称
$ nvidia-smi -L
GPU 0: Tesla V100-PCIE-32GB (UUID: GPU-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)
GPU 1: GeForce GTX TITAN X (UUID: GPU-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)
GPU 2: GeForce GTX TITAN X (UUID: GPU-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)
$ nvidia-smi --list-gpus
GPU 0: Tesla V100-PCIE-32GB (UUID: GPU-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)
GPU 1: GeForce GTX TITAN X (UUID: GPU-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)
GPU 2: GeForce GTX TITAN X (UUID: GPU-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx)
# 查看帮助信息
$ nvidia-smi -h
# 把信息写入文件中
$ nvidia-smi -f cuda.log
# 只显示指定序号的GPU
$ nvidia-smi -i 0
# 使用 -q 显示更详细的信息
$ nvidia-smi -q -i 0
$ nvidia-smi -q -i 0 -f gpu1.log
# 以 xml 格式输出信息
$ nvidia-smi -q -x
$ nvidia-smi -q -x -i 0
$ nvidia-smi -q -x -i 0 -f gpu1.log
```

# pytorch 中使用 GPU

```python
# 指定使用的GPU，应放在代码最前面
import os
os.environ["CUDA_VISIBLE_DEVICES"] = '0,1'  # 指定使用第1,2块GPU
# 查看 GPU 是否可以使用
import torch
print(torch.cuda.is_available())
> True
# 查看有多少个GPU，注意，当使用 os.environ["CUDA_VISIBLE_DEVICES"] = '0,1' 设定环境后，torch只能发现指定的GPU
torch.cuda.device_count()
> 2
# 查看指定第1个GPU的属性
> _CudaDeviceProperties(name='Tesla V100-PCIE-32GB', major=7, minor=0, total_memory=32510MB, multi_processor_count=80)
# 查看当前GPU
torch.cuda.current_device()
> 0
# 查看第1个GPU的名称
torch.cuda.get_device_name(0)
> 'Tesla V100-PCIE-32GB'
# 查看第1个GPU的算力，与torch.cuda.get_device_properties(0)对应，major=7, minor=0, 表示主算力和次算力，一般只需要看主算力，在NVIDIA官网上有每类GPU算力表
torch.cuda.get_device_capability(0)
> (7, 0)
# 使用某一个GPU
device = torch.device("cuda:0")
# 使用GPU计算
a = torch.randn(10000, 1000)
b = torch.randn(1000, 10000)
device = torch.device('cuda:0')
a = a.to(device)
b = b.to(device)
torch.matmul(a, b)
```

# NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. 错误

不知道什么情况，某次运行命令 `nvidia-smi` 时报上述错误，考虑可能是更新系统或者按照模型软件导致的，也可能是开关机导致的内核版本与安装驱动时的版本不匹配造成。解决方法如下：

```shell
# 查看 nvidia drivers 是否还在
$ whereis nvidia
nvidia: /usr/lib/x86_64-linux-gnu/nvidia /usr/lib/nvidia /usr/share/nvidia /usr/src/nvidia-460.73.01/nvidia

# DKMS(Dynamic Kernel Module Support)维护内核外的驱动程序，内核版本变动之后可以自动重新生成新的模块。
$ sudo apt-get install dkms

# 安装 nvidia drivers 版本 460.73.01，从 whereis nvidia 运行结果中查看
$ sudo dkms install -m nvidia -v 460.73.01

# 如果报错，缺少linux-headers-5.4.0-73-generic或linux-hwe-5.4-headers，需要手动下载这些DEB文件，然后安装，最后在使用dkms安装对应的 nvidia 驱动就可以了
$ sudo dpkg -i linux-hwe-5.4-headers-5.4.0-73_5.4.0-73.82_18.04.1_all.deb
$ sudo dpkg -i linux-headers-5.4.0-73-generic_5.4.0-73.82_18.04.1_amd64.deb

# 或者使用如下方法安装 linux-headers
$ sudo apt update
$ sudo apt install linux-headers-$(uname -r)

# 使用nvidia-smi测试一下，成功后可不用使用下面代码；如果不成功请运行
$ sudo dkms install -m nvidia -v 460.73.01
```



# 参考链接

1. [【CUDA】nvcc和nvidia-smi显示的版本不一致？](https://www.jianshu.com/p/eb5335708f2a)
2. [nvidia-smi 和 nvcc 结果的版本为何不一致](https://blog.csdn.net/ljp1919/article/details/102640512)
3. [nvidia-smi查看GPU的使用信息并分析](https://blog.csdn.net/wumenglu1018/article/details/103057009)
4. [实用的 NVIDIA-SMI 查询示例](https://www.jianshu.com/p/f55b86a9cc72)
5. [CUDA之nvidia-smi命令详解](https://blog.csdn.net/Bruce_0712/article/details/63683787)
6. [ubuntu关机开机后显卡挂了：报错NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. M...](https://www.jianshu.com/p/3cedce05a481)
