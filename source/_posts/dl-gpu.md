---
title: 深度学习的 GPU 环境的配置 
date: 2019.12.21
tags:
- dl
- gpu
- cuda
- nvcc
categories: 
- technology
- ubuntu
---

本篇介绍如何安装 nvidia drivers（GPU显卡驱动），cuda toolkit（GPU 通用并行计算架构） 和 cudnn（深度学习模型专用库），以 ubuntu18.04 为例，下载的软件选择 runfile(local) 版本。

<!--more-->

# nvidia drivers 

该模块就是安装显卡驱动。

[ubuntu18.04-nvidia drivers 10.2](https://www.nvidia.com/content/DriverDownload-March2009/confirmation.php?url=/tesla/440.118.02/NVIDIA-Linux-x86_64-440.118.02.run&lang=us&type=Tesla)

![](https://raw.githubusercontent.com/xujinzh/archive/master/images/ml/nvidia-driver.png)

<font size=3 color=red>**其实该步骤安装可以省略，在安装 CUDA-TOOLKIT 时可同时选择安装驱动 NVIDIA-DRIVERS** </font>

```bash
# 安装前需退出 X-SERVER
sudo su
sudo init 3
sudo sh NVIDIA-Linux-x86_64-440.118.02.run

# 当遇到安装失败时，可能是因为头文件缺少，运行下面代码即可
sudo apt update
sudo apt install linux-headers-$(uname -r)

# 查看安装是否成功
nvidia-smi
# nvidia-smi 全称是 NVIDIA System Management Interface，
# 基于 NVIDIA Management Library(NVML) 构建的命令行实用工具，
# 旨在帮助管理和监控 NVIDIA GPU 设备。
```

还有一种方法是直接使用系统命令检测合适的 GPU 显卡驱动版本，安装最适合的驱动版本

```bash
# 更新系统
sudo apt update 
sudo apt upgrade -y
# 检测
nvidia-detector
# 图形界面安装
打开 “Software and Updates”，安装相应驱动
# 或者命令行安装
sudo apt install nvidia-driver-440
# 重启系统，使驱动生效
sudo shutdown -r now
```

卸载驱动

```bash
sudo apt-get remove --purge '^nvidia-.*'
sudo apt autoremove
```



# cuda toolkit

cuda (Compute Unified Device Architecture) 模块为并行计算的加速包。CUDA™是一种由NVIDIA推出的通用并行计算架构，该架构使GPU 能够解决复杂的计算问题。按照[官方](https://link.zhihu.com/?target=https%3A//blogs.nvidia.com/blog/2012/09/10/what-is-cuda-2/)的说法是，**CUDA是一个并行计算平台和编程模型，能够使得使用GPU进行通用计算变得简单和优雅**。

[CUDA Toolkit Archive](https://developer.nvidia.com/cuda-toolkit-archive)

![](https://raw.githubusercontent.com/xujinzh/archive/master/images/ml/cuda-toolkit.png)

```bash
wget https://developer.download.nvidia.com/compute/cuda/10.2/Prod/local_installers/cuda_10.2.89_440.33.01_linux.run

# 常规安装
sudo sh cuda_10.2.89_440.33.01_linux.run

# 建议覆盖 (override) 安装，以免出错
sudo sh cuda_10.2.89_440.33.01_linux.run --toolkit --silent --override
```

在安装过程中截取其中比较重要的几个选择：

```bash
Do you accept the previously read EULA?
accept/decline/quit: accept

Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 460.81?
(y)es/(n)o/(q)uit: n # 如果在这之前已经安装好更高版本的显卡驱动就不需要再重复安装，
# 如果需要重复安装就选择 yes,此外还需要关闭图形界面。

Install the CUDA 10.2 Toolkit?
(y)es/(n)o/(q)uit: y

Enter Toolkit Location
 [ default is /usr/local/cuda-10.2 ]: # 一般选择默认即可，也可以选择安装在其他目录，
 # 在需要用的时候指向该目录或者使用软连接 link 到 /usr/local/cuda。

/usr/local/cuda-10.2 is not writable.
Do you wish to run the installation with 'sudo'?
(y)es/(n)o: y

Please enter your password: 
Do you want to install a symbolic link at /usr/local/cuda? # 是否将安装目录通过软连接的方式 link
# 到 /usr/local/cuda，yes or no 都可以，取决于你是否使用 /usr/local/cuda 为默认的 cuda 目录。
(y)es/(n)o/(q)uit: n

Install the CUDA 10.2 Samples?
(y)es/(n)o/(q)uit: n
```

```bash
# 查看安装是否成功
nvcc -V
# nvcc其实就是CUDA的编译器, 类似于gcc就是c语言的编译器
```

卸载

```bash
# 最简单的方法
sudo rm -rf /usr/local/cuda-10.2

# 高级方法
# Uninstall just nvidia-cuda-toolkit
sudo apt-get remove nvidia-cuda-toolkit

# Uninstall nvidia-cuda-toolkit and it's dependencies
sudo apt-get remove --auto-remove nvidia-cuda-toolkit

# Purging config/data
sudo apt-get purge nvidia-cuda-toolkit 
# or 
sudo apt-get purge --auto-remove nvidia-cuda-toolkit
```



# cudnn

cudnn 模块其实就是一个专门为深度学习计算设计的软件库，里面提供了很多专门的计算函数，如卷积等。

cudnn 下载时需要登录，下载对应的版本：[cudnn](https://developer.nvidia.com/rdp/cudnn-download)

![](https://raw.githubusercontent.com/xujinzh/archive/master/images/ml/cudnn.jpeg)

然后，参考官网 [NVIDIA CUDNN DOCUMENTATION](https://docs.nvidia.com/deeplearning/cudnn/install-guide/index.html#installlinux) 安装

```bash
tar -xzvf cudnn-x.x-linux-x64-v8.x.x.x.tgz
sudo cp cuda/include/cudnn*.h /usr/local/cuda/include 
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda/lib64 
sudo chmod a+r /usr/local/cuda/include/cudnn*.h /usr/local/cuda/lib64/libcudnn*
```

注意：

1. 需要把 CUDA_HOME 添加到环境变量，一般CUDA_HOME=/usr/locdal/cuda-*/bin，我的配置如下

   ```bash
   export CUDA_HOME=/usr/local/cuda
   export LD_LIBRARY_PATH=${CUDA_HOME}/lib64:${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
   export PATH=$CUDA_HOME/bin:$PATH
   ```

   

2. 所有都安装成功后，可以运行 `nvidia-smi` 和 `nvcc --version` 来检验是否完美安装

# 莫名 nvidia-smi 无法使用的解决方法

在平常使用时，偶尔会出现命令 nvidia-smi 无法使用的情况，这种一般是 Linux 内核更新导致的，一种解决方法如下：

```bash
# 安装头文件
sudo apt install linux-headers-$(uname -r)
# 查看当前安装的 nvidia 版本
whereis nvidia
# 再次安装，如版本 440.33.01，请改成自己对于的版本号
sudo dkms install -m nvidia -v 440.33.01
```



# 建议参考官网安装

## [NVIDIA CUDA Installation Guide for Linux](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#abstract)

https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#post-installation-actions

# 参考链接

1. [gpu 的驱动，cuda，cudnn的关系](https://benpaodewoniu.github.io/2019/12/07/nvidia0/)
2. [Ubuntu下安装CUDA](https://www.cnblogs.com/whenyd/p/7787447.html)
3. [显卡，显卡驱动,nvcc, cuda driver,cudatoolkit,cudnn到底是什么？](https://zhuanlan.zhihu.com/p/91334380)
4. [Ubuntu – Removing Nvidia CUDA Toolkit and installing new one](https://itectec.com/ubuntu/ubuntu-removing-nvidia-cuda-toolkit-and-installing-new-one/)
5. [**Ubuntu系统---系统驱动丢失、Kernel内核卸载、禁止更新**](https://www.cnblogs.com/carle-09/p/11363020.html)

