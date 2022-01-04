---
title: 解决使用 Python 的 matplotlib 画图时中文显示为乱码的问题
mathjax: true
author: Jinzhong Xu
date: 2021-05-06 09:46:29
tags:
- python
- matplotlib
- Chinese
categories:
- technology
- python
---

在使用 `Python` 的 `matplotlib` 画图时如果设置了标题或者坐标轴名称为中文时，显示图片上中文出现乱码或四边形，不能正常显示我们需要的中文字，体验非常不好。下面给出一个解决方法。本篇以 Linux （Debian）系统为例。

<!--more-->

#  下载中文字体

可以在 [wfonts](https://www.wfonts.com/font/simhei) 下载，也可以在其他任何能够下载到 `SimHei.ttf` 的地方下载。

# 把字体文件放在合适的位置

把字体放在 `matplotlib` 能够发现的位置，如何指导正确的位置

```python
➜  ~ ipython
Python 3.8.5 (default, Sep  4 2020, 07:30:14) 
Type 'copyright', 'credits' or 'license' for more information
IPython 7.23.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: import matplotlib

In [2]: matplotlib.matplotlib_fname()
Out[2]: '/usr/local/miniconda/lib/python3.8/site-packages/matplotlib/mpl-data/matplotlibrc'
```

从上面可以发现一个目录：`'/usr/local/miniconda/lib/python3.8/site-packages/matplotlib/mpl-data/'`，在该目录下有个文件夹

```bash
➜  ~ ls /usr/local/miniconda/lib/python3.8/site-packages/matplotlib/mpl-data
fonts  images  matplotlibrc  plot_directive  sample_data  stylelib
➜  ~ ls /usr/local/miniconda/lib/python3.8/site-packages/matplotlib/mpl-data/fonts
afm  pdfcorefonts  ttf
```

把下载的 `SimHei.ttf` 拷贝到 `ttf` 文件夹下

# 清除旧缓存

一般需要先清除掉旧的 `matplotlib` 缓存后才能正常使用，缓存一般在家目录下，可以通过如下方法获得缓存地址

```python
➜  ~ ipython 
Python 3.8.5 (default, Sep  4 2020, 07:30:14) 
Type 'copyright', 'credits' or 'license' for more information
IPython 7.23.0 -- An enhanced Interactive Python. Type '?' for help.

In [1]: import matplotlib

In [2]: matplotlib.get_cachedir()
Out[2]: '/home/jinzhongxu/.cache/matplotlib'
```

发现我的确实在家目录下，因此直接去家目录删除即可

```bash
➜  ~ ls .cache            
jedi  matplotlib  yarn
➜  ~ ls .cache/matplotlib 
fontlist-v330.json
➜  ~ rm -r ~/.cache/matplotlib
```

# 画图时使用中文

配置好中文字体后，只需要在每次画图时指定字体就可以正常显示中文字符了。

```python
import matplotlib.pyplot as plt
import numpy as np

plt.rcParams["font.sans-serif"] = ["SimHei"]  # 用来显示中文
plt.rcParams["axes.unicode_minus"] = False  # 用来显示负号

x = np.linspace(-5, 5, 101)
y = np.cos(x)
plt.plot(x, y, "r")
plt.title(u"余弦")
plt.show()
```

# 一劳永逸

从上面可知，每次画图都需要在代码中配置中文字体才能正常显示中文字符，下面通过设置可以直接使用中文字体，那就是修改上面的配置文件

```bash
vim /usr/local/miniconda/lib/python3.8/site-packages/matplotlib/mpl-data/matplotlibrc

# 把前面的注释去掉
font.family         : sans-serif
# 去掉注释，并添加字体名 SimHei 在第一个位置
font.sans-serif     : SimHei, Bitstream Vera Sans, ...
# 去掉注释，并修改 True 为 False
axes.unicode_minus  : False
```

# 扩展-画图时隐藏图框

```python
import matplotlib.pyplot as plt
import numpy as np

# 画心形线 
a = 3
x = np.linspace(-(a + 1) * np.pi / 4, (a + 1) * np.pi / 4, 301)
y1 = a * (1 + np.cos(x)) * np.cos(x)
y2 = a * (1 + np.cos(x)) * np.sin(x)

# 隐藏边框
fig, ax = plt.subplots()
for key, spine in ax.spines.items():
    # if key == "left" or key == "right" or key == "top" or key == "bottom":
    #     spine.set_visible(False)
    spine.set_visible(False)

# 注意坐标
plt.plot(y2, -y1)
plt.xticks([])
plt.yticks([])
plt.axis("equal")
plt.show()
```



# 参考链接

1. [matplotlib对中文的支持（Font family [‘sans-serif’] not found.Falling back to DejaVu Sans）](https://www.codenong.com/cs106003277/)

2. [ubuntu系统下matplotlib中文乱码问题](https://blog.csdn.net/jeff_liu_sky_/article/details/54023745)

   