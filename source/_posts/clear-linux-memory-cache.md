---
title: 清理 Linux 系统内存 Cache
mathjax: true
author: Jinzhong Xu
date: 2021-06-20 19:04:06
tags:
- linux
- ubuntu
- centos
categories:
- technology
- linux
---

每个 Linux 系统都有三个选项可以在不中断任何进程或服务的情况下清除缓存。本篇代码以 root 身份在 Ubuntu 18.04演示。

<!--more-->

# 仅清除页面缓存(Clear PageCache only)

```bash
sync; echo 1 > /proc/sys/vm/drop_caches
```

# 清除目录项和inode(Clear dentries and inodes)

```bash
sync; echo 2 > /proc/sys/vm/drop_caches
```

# 清理三者(Clear PageCache, dentries and inodes)

```bash
sync; echo 3 > /proc/sys/vm/drop_caches
```

同步 (`sync`) 将刷新文件系统缓冲区。命令以 “;” 分隔依次运行。在执行序列中的下一个命令之前，shell 等待每个命令终止。正如内核文档中提到的，写入 drop_cache 将清除缓存而不杀死任何应用程序/服务，命令 echo 正在执行写入文件的工作。

如果您必须清除磁盘缓存，第一个命令在企业和生产中是最安全的“...echo 1 > ...”。只会清除 PageCache。不建议在生产中使用“...echo 3 >”上方的第三个选项，直到您知道自己在做什么，因为它会清除 PageCache、dentries 和 inode。

# 定时清理缓存

```bash
vim clearcache.sh
```

写入如下内容

```bash
#!/bin/bash
# Note, we are using "echo 3", but it is not recommended in production instead use "echo 1"
sync
echo 3 > /proc/sys/vm/drop_caches
```

```bash
chmod 755 clearcache.sh
```

以 root 身份创建定时清理任务

```bash
crontab -e
```

写入如下内容，每天0点，12点，18点的0分清理缓存

```bash
0  0,12,18  *  *  *  /path/to/clearcache.sh
```

<font size=5 color=red>不建议定时清理RAM</font>

# 参考链接

1. [How to Clear RAM Memory Cache, Buffer and Swap Space on Linux](https://www.tecmint.com/clear-ram-memory-cache-buffer-and-swap-space-on-linux/)
2. [在 Linux 上如何清除内存的 Cache、Buffer 和交换空间](https://colobu.com/2015/10/31/How-to-Clear-RAM-Memory-Cache-Buffer-and-Swap-Space-on-Linux/)

