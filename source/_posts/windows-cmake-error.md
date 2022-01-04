---
title: Windows 10 Cmake error 解决方法
author: Jinzhong Xu
mathjax: true
date: 2020-08-03 17:36:43
tags:
- windows
- cmake
categories: technology
---

使用 Windows10 开发 Python 程序时，有时候需要编译安装一些模块，如 dlib 等，需要用到 Cmake 在本地编译安装，但有时候总是会出现

```powershell
Error in Cmake “The C compiler identification is unknown” 
```

<!--more-->

这时候，主要是因为 Windows10 编译工具未有安装导致的，解决方法就是安装它。

## 安装 Cmake

下载 Cmake，下载地址：https://cmake.org/download/

下载后安装，并配置环境变量

## 安装 visual studio build tools

下载 visual studio build tools，下载地址：https://visualstudio.microsoft.com/zh-hans/visual-cpp-build-tools/

下载后，安装，可能需要一段时间安装

## 测试安装 dlib

安装完 Cmake 和 visual studio build tools 后，测试编译安装模块

```powershell
pip install dlib
```

