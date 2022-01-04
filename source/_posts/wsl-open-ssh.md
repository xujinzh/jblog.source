---
title: WSL 开启SSH
author: Jinzhong Xu
mathjax: true
date: 2020-07-24 17:12:45
tags:
- windows
- ssh
- wsl
categories: technology
---

​	Windows10 安装Linux子系统后，如何开启SSH服务，使得远程可以连接该WSL呢？下面介绍这一过程。

<!--more-->

## Windows10 打开22端口

1. Windows键 + r 打开运行，输入 control，回车打开控制面板
2. 找到“系统和安全”，注意右上角查看方式为“类别”，点进去找到防火墙。或者直接改变查看方式为“大图标”，找到防火墙
3. 进入防火墙，找到左边的“高级设置”
4. 找到左上角的“入站规则”，点击进入，找到右上角的新建规则，选端口，然后输入需要打开的端口，这里是22，然后是运行连接，最后，填写名称SSH
5. 出站规则同样

## WSL 安装SSH服务

```bash
sudo apt install openssh-server openssh-client
```

开启sshd

```bash
sudo /etc/init.d/ssh restart

# 如果出现如下内容，说明没有开启成功，需要生成相应的密钥
$ sudo /etc/init.d/ssh restart
Could not load host key: /etc/ssh/ssh_host_rsa_key
Could not load host key: /etc/ssh/ssh_host_ecdsa_key
Could not load host key: /etc/ssh/ssh_host_ed25519_key
* Restarting OpenBSD Secure Shell server sshd
 Could not load host key: /etc/ssh/ssh_host_rsa_key
Could not load host key: /etc/ssh/ssh_host_ecdsa_key
Could not load host key: /etc/ssh/ssh_host_ed25519_key

# 查看/etc/ssh目录下文件名，发现真的确实相应的密钥
$ ls -l /etc/ssh
total 552
-rw-r--r-- 1 root root 553122 Mar  4  2019 moduli
-rw-r--r-- 1 root root   1580 Mar  4  2019 ssh_config
-rw-r--r-- 1 root root    338 Mar  5 00:01 ssh_import_id
-rw-r--r-- 1 root root   3261 Jul 24 16:57 sshd_config

# 生成密钥，注意以root身份，或者可以使用 sudo dpkg-reconfigure openssh-server，参考连接2
ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key
ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key

# 再次查看/etc/ssh目录
$ ls -l /etc/ssh
total 556
-rw-r--r-- 1 root root 553122 Mar  4  2019 moduli
-rw-r--r-- 1 root root   1580 Mar  4  2019 ssh_config
-rw------- 1 root root    227 Jul 24 17:09 ssh_host_ecdsa_key
-rw-r--r-- 1 root root    182 Jul 24 17:09 ssh_host_ecdsa_key.pub
-rw------- 1 root root    411 Jul 24 17:09 ssh_host_ed25519_key
-rw-r--r-- 1 root root    102 Jul 24 17:09 ssh_host_ed25519_key.pub
-rw------- 1 root root   1675 Jul 24 17:08 ssh_host_rsa_key
-rw-r--r-- 1 root root    402 Jul 24 17:08 ssh_host_rsa_key.pub
-rw-r--r-- 1 root root    338 Mar  5 00:01 ssh_import_id
-rw-r--r-- 1 root root   3261 Jul 24 16:57 sshd_config

# 最后，开启sshd
sudo /etc/init.d/ssh restart
```

注意，如果使用putty等连接wsl出现：Remote side unexpectedly closed network connection，那么很可能就是上面的密钥导致的sshd无法启动的问题。

## 无需回车生成秘钥

```bash
ssh-keygen -t rsa -N '' -f ~/.ssh/id_rsa <<< y
```

## 参考链接

1. [启动sshd时，报“Could not load host key”错](https://www.cnblogs.com/netonline/p/7410586.html)
2. [修复“sshd error: could not load host key”](https://www.jianshu.com/p/bf7daa4250b6)