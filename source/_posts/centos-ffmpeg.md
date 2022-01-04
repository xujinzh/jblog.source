---
title: Centos安装ffmpeg
author: Jinzhong Xu 
date: 2020-03-10 12:04:04
tags:
- centos
- ffmpeg
categories: technology
---

Centos 安全稳定，但是，对于ffmpeg却没有通过yum install ffmpeg直接按照的支持。下面给出一种可行的安装方法：**Static Prebuilt Install of FFMpeg.** 这里以Centos 7为例。

<!--more-->

# 下载安装脚本

```bash
wget https://raw.githubusercontent.com/q3aql/ffmpeg-install/master/ffmpeg-install
```

# 安装

```bash
chmod  a+x ffmpeg-install && ./ffmpeg-install --install release
```

# 查看ffmpeg版本信息

```bash
ffmpeg -version
```

# 其他补充

Centos 7 不能直接使用tar 解压缩 x.tar.bz2?

```bash
sudo yum install bzip2
```

