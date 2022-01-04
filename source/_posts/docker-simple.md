---
title: docker 简单介绍
mathjax: true
author: Jinzhong Xu
date: 2020-11-05 11:26:08
tags:
- docker
categories:
- technology
- ubuntu
---

docker 容器技术使得开发测试非常方便，自2013年发布至今，一直广受瞩目。下面介绍2020年08月以后，如何安装使用docker，本篇以 Windows 10 wsl 2 Ubuntu-20.04 为例。

<!--more-->

# docker安装

```bash
sudo apt-get update
# 安装 apt 依赖包，用于通过HTTPS来获取仓库
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
# 添加 Docker 的官方 GPG 密钥：
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# 验证秘钥
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
   "deb [arch=amd64] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/ \
  $(lsb_release -cs) \
  stable"
 
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo service docker status
sudo service docker restart
# 把常用用户添加到docker组，这样运行docker命令时不需要添加sudo，但需重启终端。注意，service命令时仍然需要sudo
sudo usermod -a -G docker jinzhongxu
sudo docker run hello-world
```

# docker使用

因某些原因，需要给docker添加国内镜像，方法如下

```bash
sudo vim /etc/docker/daemon.json
# 添加如下内容
{
  "registry-mirrors": ["https://registry.docker-cn.com",
                        "http://hub-mirror.c.163.com",
                        "https://docker.mirrors.ustc.edu.cn",
                        "https://docker.mirrors.ustc.edu.cn",
                        "https://cr.console.aliyun.com/"],
  "insecure-registries": [],
  "debug": true,
  "experimental": false
}
# 测试
docker pull tensorflow/tensorflow:latest
```

docker image 管理

```bash
# 查看下载到本地的镜像文件
docker image ls
# 删除不需要的本地镜像文件
docker image rm (IMAGE ID)
```

docker container 管理

```bash
# 查看运行的容器
docker container ls
# 查看所有运行过的容器，包括正在运行的和停止运行的
docker ps -a
# 把运行的容易关闭
docker stop {CONTAINER ID}
# 把停止运行的容器打开
docker start {CONTAINER ID}
# 进入运行中的ubuntu容器
docker exec -it {CONTAINER ID} /bin/bash
# 以后台方式打开新的容器并命名
docker run -itd --name='centos' {IMAGE ID} /bin/bash
# 删除不需要的容器
docker container rm {CONTAINER ID}
```

注意，CONTAINER ID 和 IMAGE ID 只需要输入前几个字符，只要能够唯一识别它们。

# 参考链接

1. [docker国内镜像源](https://www.cnblogs.com/StivenYang/p/13843397.html)

2. [Docker 入门教程](https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)

   