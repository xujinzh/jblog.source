---
title: Linux 压缩(解压缩)命令 zip(unzip) 和 rar(unrar)
date: 2019-12-20 19:17:30
tags:
- debian
- shell
- linux
- zip
- rar
categories: 
- technology
- linux
---

zip 和 rar 都是常有的压缩命令，特别是在 Windows 上压缩的 zip 或 rar 文件，上传到 Linux 系统上时，需要解压缩，下面介绍如何在 Linux 上使用命令 zip (unzip) 和 rar (unrar) 进行压缩和解压缩。

<!--more-->

# <font size=7 color=red>zip压缩 </font>

## zip 压缩多个文件或目录到一个压缩包

```bash
zip -r file.zip wordcount.jar /home/jayzonxu/flink
```

##  zip 压缩同后缀的文件到一个压缩包

```bash
zip file.zip *.jpeg
```

# <font size=7 color=orange>unzip 解压缩</font>

## unzip 解压缩文件

```bash
unzip file.zip
```

# <font size=7 color=red>rar 压缩</font>

## 编译安装 rar

```bash
wget https://www.rarlab.com/rar/rarlinux-x64-6.0.2.tar.gz
tar -xzf rarlinux-x64-6.0.2.tar.gz
cd rar
sudo make
```



##  把所有文件压缩到 all.rar

```bash
rar a file.rar *.jpeg filename.zip wordcount.jar /home/jayzonxu/flink
```

# <font size=7 color=orange>unrar 解压缩</font>

## unrar 解压缩文件

```bash
unrar e file.rar
unrar x file.rar
```



# 参考链接

1. [Linux中如何安装RAR](https://www.cnblogs.com/findumars/p/8244997.html)

