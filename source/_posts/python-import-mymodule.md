---
title: 在 Python 中导入自己编写的模块
mathjax: true
author: Jinzhong Xu
date: 2021-04-10 13:12:27
tags:
- python
- import 
- module
categories:
- technology
- python
---

在 Python 中导入包可以直接使用 import，非常简便。但是，对于自己编写的 Python 模块不能简单的使用该方法导入，特别是想在任意目录下使用自己编写的模块时。如何实现向 Python 安装的第三方包一样直接使用 import 在任意目录下导入自己的模块呢？下面介绍几个方法。假设自己模块的目录为：/home/jinzhongxu/mymodule

<!--more-->

# 使用 module.pth

按照下面的顺序执行

```bash
# 查看 Python 包目录
ipython
In [1]: import site

In [2]: site.getsitepackages()
Out[2]: ['/usr/local/miniconda/lib/python3.8/site-packages']

# 新建 module.pth, 注意以 pth 结尾
echo '/home/jinzhongxu/mymodule' >> module.pth 

# 把 module.pth 放入 Python 包目录
mv module.pth /usr/local/miniconda/lib/python3.8/site-packages/.
```

这样就可以使用了。

# 使用 sys

这种方法是在每次调用自己写的模块时，把模块的路径添加的代码运行环境中

```python
import sys
sys.path.append("/home/jinzhongxu/mymodule")  //设置自定义模块路径
```

这样就可以使用了。

# 参考连接

1. [python导入自定义模块方法](https://www.jianshu.com/p/6692b48c7295)
2. [将文件夹加入到sys.path](https://python3-cookbook.readthedocs.io/zh_CN/latest/c10/p09_add_directories_to_sys_path.html) 