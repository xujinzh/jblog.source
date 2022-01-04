---
title: Centos 安装 htop
date: 2020-01-02 19:28:45
tags:
- linux
- centos
categories: technology
---

htop 是一个非常优秀好用的命令来查看系统运行情况，在Ubuntu上可以非常简单的使用命令

<!--more-->

```bash
sudo apt update && apt install htop
```

安装htop

但是，在Centos 7等上却不能简单的使用一条命令安装htop，下面给出完整的命令来在Centos 7上安装htop

```bash
sudo yum install epel-release -y

sudo yum update -y

sudo yum install htop -y
```

到此，就正常安装了htop在Centos 7上，可以在终端运行 htop 命令来查看是否安装成功。

