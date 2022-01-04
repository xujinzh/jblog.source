---
title: 因 sudoers 出错无法使用 sudo 的解决方法
mathjax: true
author: Jinzhong Xu
date: 2021-03-21 18:22:32
tags:
- linux
- sudo
categories:
- technology
- linux
---

Linux 上修改 /etc/sudoers 导致文件出错，此时只要能够修改该文件的错误即可，但是普通用户 jinzhongxu 无法使用 sudo 命令，且无法使用 root 用户登陆。因此，需要能够进行 /etc/sudoers 修改，并更正错误即可。以下给出一种解决方法。

<!--more-->

# 解决方案

1. 使用 ssh 连接普通用户 jinzhongxu，并打开两个终端 shell：t1, t2

2. 在 t1 输入命令：

   `echo $$`

   并把返回的数字拷贝下来

3. 在 t2 输入命令：

   `pkttyagent --process 拷贝的数字 `
   
4. 再在 t1 中输入

   `pkexec visudo`

   此时 t1也会卡住

5. 再切换到 t2，输入用户 jinzhongxu 的密码

6. 再次切换到 t1，发现此时正在打开 /etc/sudoers 等到修改，修改 /etc/sudoers 中出错的内容并保存。

# 参考文献

1. [Ubuntu改坏sudoers后无法使用sudo的解决办法](https://juejin.cn/post/6844904070545670158)

