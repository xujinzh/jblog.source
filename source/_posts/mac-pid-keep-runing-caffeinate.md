---
title: 在 Mac 上使用 caffeinate 保持进程运行
mathjax: true
author: Jinzhong Xu
date: 2021-08-31 22:08:24
tags:
- mac
- caffeinate
categories:
- technology
- mac
---

当把 Mac Book Pro 的显示器盖合上后，有些进程会无法继续运行。下面介绍一种方法能让进程一直运行下去。

<!--more-->

```bash
# 该命令能让进程一直保持运行
caffeinate -w <进程号>

# 只保持3600秒
caffeinate -t 3600
```

如何获取进程号

```bash
# 添加 -v grep是为了避免匹配到 grep 进程
ps -ef | grep "ssh -L" | grep -v grep | awk '{print $2}'

# 名称首字母加[]的目的是为了避免匹配到 awk 自身的进程
ps -ef | awk '/[n]ame/{print $2}'

# 只使用 x 参数，则 pid 位于第一位
ps x | awk '/[n]ame/{print $1}'

# 简单获取 pid
pgrep -f name

# 杀掉 pid
pkill -f name

# 如果是可执行程序
pidof name
```

实例，保持 ssh 合盖不断开

```bash
pgrep -f 'ssh -L' | xargs caffeinate -w

# 或者在后台运行
nohup pgrep -f 'ssh -L' | xargs caffeinate -w &
```

# 参考链接

1. [caffeinate 知乎](https://www.zhihu.com/question/312052061)
2. [linux shell 根据进程名获取pid](https://blog.csdn.net/baidu_33850454/article/details/78568392)
