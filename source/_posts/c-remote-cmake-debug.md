---
title: Clion 运行 C 出现 cmake-build-debug 找不到的解决方法
mathjax: true
author: Jinzhong Xu
date: 2021-04-08 09:32:06
tags:
- c
- cmake
- clion
categories:
- technology
- c/c++
---

使用 Clion 连接远程服务器，利用服务器上的 c 编译器、make、cmake 进行 C 开发非常方便高效，但是，有时候编译时会出现 cmake-build-debug 找不到，导致无法编译运行 C 代码。下面给出解决方法。

<!--more-->

使用远程服务器时，已经自动将本地的 CMakeLists.txt、main.c 和 其他 .c 文件同步到服务器上，但是，无法同步本地的 cmake-build-debug 文件夹，该文件夹是编译运行 C 程序的。尝试通过两种方法解决。

# 手动远程服务器上生成

首先通过 SSH 连接到远程服务器上，并 cd 到 C 工程目录，然后，创建文件夹 cmake-build-debug

```bash
mkdir cmake-build-debug
```

最后，使用服务器的 cmake 生成 make 文件

```bash
cd cmake-build-debug
cmake ..
```

# 在 Clion 上自动生成

其实 Clion 上给出了自动生成 make file 的工具。

在 Clion 窗口最下面一行，找到 CMake，然后点击 CMake 窗口左上角的同步（Reload CMake Project），此时，会在远程服务器上自动生成 make file，即 cmake-build-debug 文件夹。



