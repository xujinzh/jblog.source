---
title: 命令行运行 Python 代码时传参数
mathjax: true
author: Jinzhong Xu
date: 2021-10-25 14:10:53
tags:
- python
- shell
categories:
- technology
- python
---

Python 代码能够以多种形式运行，如在 JupyterLab 中交互式方式、在 PyCharm 中以工程代码形式运行。有时，我们还需要在终端或CMD 以命令行的形式运行。这些运行方式都能够实现 Python 代码的运行。对于需要自定义参数的代码，在 JupyterLab 中我们可以把参数写入单元格内，在 PyCharm 中我们可以将参数写入配置文件内，那么在终端或CMD运行 Python 代码时，如何传入参数呢，下面介绍两种方法。

<!--more-->

# 利用 sys 包

编写名为 `shellSysArgv.py` 的模块

```python
import sys

# 测试命令行运行 python 代码时传参数

def main(x, y):
    print(f"x={x}, y={y}")
    print("done!")
    
    
if __name__=="__main__":
    x = sys.argv[1]
    y = sys.argv[2]
    main(x, y)
```

在 shell 中调用

```bash
$ python shellSysArgv.py 1 2        
x=1, y=2
done!
```

# 利用 argparse 包

编写名为 `shellArgParse.py` 的模块

```python
import argparse

# 测试命令行运行 python 代码时传参数，同时对参数进行限制

def main(x_data, y_data):
    print(f"x={x_data}, y={y_data}")
    print("done!")
    
    
if __name__=="__main__":
    ap = argparse.ArgumentParser()
    ap.add_argument('-x', '--x_data', required=True, type=str, default='1', help='x axis')
    ap.add_argument('-y', '--y_data', required=True, type=str, default='2', help='y axis')
    args = vars(ap.parse_args())
    
    x_data = args['x_data']
    y_data = args['y_data']
    main(x_data, y_data)
```

在 shell 中调用

```bash
$ python shellArgParse.py -x=1 -y=2
x=1, y=2
done!
```

# 参考链接

1. [命令行运行Python脚本时传入参数的三种方式](https://blog.csdn.net/weixin_35653315/article/details/72886718)

