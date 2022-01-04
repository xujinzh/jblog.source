---
title: 在 Python 中调用 Shell 命令
mathjax: true
author: Jinzhong Xu
date: 2021-05-25 16:01:48
tags:
- python
- shell
categories:
- technology
- python
---

使用 Python 调用 Shell 命令能够非常方便的使用原生 Shell 命令，扩大 Python 的使用作用范围。进一步说明了 Python 作为胶水语义的特性。

<!--more-->

# Python 中使用 shell 命令

## 使用 ！号

仅在 Jupyter Notebook 中可以使用。对于一些简单命令，如`ls, pwd`等其实不使用`!`也是可以的， 但是稍微复杂的 shell 命令是不行的，建议都使用 `!` 作为 shell 命令的开头在 Jupyter Notebook 中使用。

```python
!ls
>...
username = !whoami
username
> 'jinzhongxu'
username[0]
> 'jinzhongxu'
type(username)
> IPython.utils.text.SList
!ls /home/{username[0]}
> Desktop      examples.desktop	 Music	    Videos
  Documents    Pictures   Templates
list(username)[0]
> 'jinzhongxu'
!ls /home/{list(username)[0]}
> Desktop      examples.desktop	 Music	    Videos
  Documents    Pictures   Templates
```

## 使用 os.popen 函数

即可以在 Jupyter Notebook 中使用，也可以在 Pycharm 中使用。

```python
import os
command = 'whoami'
username = os.popen(command).read().split('\n')
type(username)
> list
username
> ['jinzhongxu', '']
username[0]
> 'jinzhongxu'
command_ls = f'ls /home/{username[0]}'
result = os.popen(command_ls).read().split('\n')
result
> ['Desktop',
   'Documents',
   'examples.desktop',
   'Music',
   'Pictures',
   'Templates',
   'Videos',
   '']
```

## 使用 subprocess.Popen 函数

即可以在 Jupyter Notebook 中使用，也可以在 Pycharm 中使用。

```python
import subprocess
p = subprocess.Popen("ls .", shell=True, stdout=subprocess.PIPE)
[name.decode().split('\n')[0] for name in p.stdout.readlines()]
```

## 使用 os.system 函数

即可以在 Jupyter Notebook 中使用，也可以在 Pycharm 中使用。

注意，该函数直接运行 shell 命令，但返回值是 0，不像 os.popen 和 subprocess.Popen 返回值是 shell 命令运行后的打印结果。

```python
os.system("ls")
```

# Python 脚本中使用 shell 命令

在 Python 脚本使用 shell 命令不能使用 "!" 号，可以使用 os.popen、subprocess.Popen、os.system 函数。

注意，os.popen 返回字符串中每一个条目后面带有 '\n'; 而 subprocess.Popen 不仅带有 '\n'，还是以字节编码的，需要写解码 decode(); os.system 返回值为 0.

# 参考链接

1. [Python os.popen() 方法](https://cloud.tencent.com/developer/article/1486996)
2. [`subprocess` -- 子进程管理](https://docs.python.org/zh-cn/3/library/subprocess.html)
