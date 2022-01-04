---
title: 解决SSH连接CentOS速度慢的问题
mathjax: true
author: Jinzhong Xu
date: 2021-01-29 10:19:08
tags:
- ssh
- centos
categories:
- technology
- linux
---

使用 SSH 登录 CentOS 时，总是比登录 Ubuntu 等系统慢。解决方法就是如下设置。本文以 CentOS 7 为例。

<!--more-->

# 修改 SSH 配置

1. 登录系统

2. 打开 SSH 配置文件：

   ```bash
   vi /etc/ssh/sshd_config
   ```

3. 修改 UseDNS 为如下

   ```bash
   UseDNS no
   ```

4. 修改 GSSAPIAuthentication 为如下

   ```bash
   GSSAPIAuthentication no
   ```

5. 重启 SSHD 服务

   ```bash
   service sshd restart
   # 或者
   systemctl restart sshd
   ```

   

# 参考链接

1. [centos ssh连接登录慢解决](https://blog.csdn.net/qq_30264689/article/details/89314555)
2. [centos7解决ssh登录速度慢的问题](https://blog.csdn.net/leeyisong/article/details/81558478)