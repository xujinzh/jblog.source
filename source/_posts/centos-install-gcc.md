---
title: CentOS 编译安装 gcc
mathjax: true
author: Jinzhong Xu
date: 2021-02-03 14:50:01
tags:
- gcc
- centos
- linux
- make
categories:
- technology
- linux
---

gcc 是 Linux 系统的核心模块，同时，它可以编译使用 C，C++ 等语言编写的源代码。但是，在某些系统上自带的 gcc 版本过低，导致一些软件无法正常安装和运行。本篇介绍在 CentOS 上如何编译安装最新版或者高版本的 gcc，默认在 root 用户下运行命令。

<!--more-->

# 下载最新版 gcc

下载 gcc 可以在下面的官方网址下载，里面有最新版的 gcc 源码

[GCC官方网址](http://ftp.gnu.org/gnu/gcc/)

或者使用 Git 克隆最新版

git clone https://gcc.gnu.org/git/gcc.git

如果官网下载慢，可以采用如下的镜像网址

[中国科技大学镜像网址](https://mirrors.ustc.edu.cn//gnu/gcc/)

[华中科技大学镜像网址](http://mirror.hust.edu.cn/gnu/gcc/)

[南京大学镜像网址](http://mirrors.nju.edu.cn/gnu/gcc/)

[清华大学镜像网址](https://mirrors.tuna.tsinghua.edu.cn/gnu/gcc/)

我这里下载的测试版本是 gcc-10.1.0，

```bash
wget -c http://ftp.gnu.org/gnu/gcc/gcc-10.1.0/gcc-10.1.0.tar.gz
# 或者下载到指定目录
wget -c http://ftp.gnu.org/gnu/gcc/gcc-10.1.0/gcc-10.1.0.tar.gz -p /root/.
tar -xzvf gcc-10.1.0.tar.gz
```

# 安装工具软件

因为下载的是 gcc 源码，因此，需要先编译才能安装。所以，需要在 CentOS 上需要安装一下工具软件，方法如下

```bash
yum -y install wget bzip2 gcc gcc-c++ glibc-headers
```

# 编译安装最新版 gcc

```bash
cd /root/gcc-10.1.0
# 下载依赖包
./contrib/download_prerequisites
# 创建文件夹，存储编译中产生的临时文件
mkdir build
cd build/
# 配置编译
../configure --prefix=/usr/local/gcc-10.1.0 --enable-bootstrap --enable-checking=release --enable-languages=c,c++ --disable-multilib
# 进行编译，经测试花销3小时30分钟左右
make
# 安装，大约3-5分钟左右
make install
# 配置为默认 gcc
echo -e '\nexport PATH=/usr/local/gcc-10.1.0/bin:$PATH\n' >> /etc/profile.d/gcc.sh && source /etc/profile.d/gcc.sh
ln -sv /usr/local/gcc-10.1.0/include/ /usr/include/gcc
# 配置生效
ldconfig -v
# 导出验证
ldconfig -p | grep gcc
# 查看 gcc 版本号
gcc -v
```

# 参考链接

1. [CentOS 7.6 编译安装最新版本GCC 9.2.0 实录](https://developer.aliyun.com/article/718551)

2. [Linux升级gcc到最新版本--gcc-9.1.0](https://blog.csdn.net/weixin_42090356/article/details/90678158)

3. [安装最新的GCC](https://blog.csdn.net/21aspnet/article/details/105708122)

   