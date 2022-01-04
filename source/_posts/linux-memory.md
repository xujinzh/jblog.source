---
title: 查看内存消耗排序情况
date: 2019.12.21
tags:
- linux
- ubuntu
categories: technology
---

在终端运行如下命令

```bash
ps -aux | sort -k4nr | head -n 25
```

查看占用内存从大到小的25个进程