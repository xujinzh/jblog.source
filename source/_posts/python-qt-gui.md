---
title: 利用 PyQt5 开发 Python 图形用户程序
mathjax: true
author: Jinzhong Xu
date: 2021-11-28 11:27:55
tags:
- python
- pyqt5
- gui
- shell
categories:
- technology
- python
---

Python 作为一种高级编程语言，简洁高效，开发效率高。将 Python 与 JupyterLab 结合使用非常方便，也是我之前常采用的形式。但本篇主要介绍用 Python 开发桌面等应用程序，利用图形界面实现与用户交互。本篇将包含 Miniconda, PyQt5, pyqt5-tools, Qt Designer, pyinstaller 等。

<!--more-->

# 安装需要的包

首先，从[官网](https://docs.conda.io/en/latest/miniconda.html)下载 Miniconda，配置好开发环境，这时就可以使用 Python 了。

其次，从 [Python packages 官网](https://pypi.org/) 安装 PyQt5 和 pyqt5-tools（PyQt6 在 2021年1月份已经发布，截止本篇文章，已经到 6.2.1，大家可以自己选择使用）。

```python
pip install PyQt5
pip install pyqt5-tools
```

到目前，所安装的包已经足够开发 Python 图形用户程序，但设计图形界面不是那么直观，可利用下面的 Qt Designer  进行图形设计界面。

然后，从网址 [fman build system](https://build-system.fman.io/qt-designer-download) 下载 Qt Designer，包含有 Windows 版和 Mac 版。下载需要的版本，并安装。在 Windows 上利用 Miniconda 安装过 PyQt5 和 pyqt5-tools 后，默认会在路径 `miniconda\Lib\site-packages\qt5_applications\Qt\bin`下包含有 Qt Designer 工具，只需要把它添加到 PyCharm 中即可，方法如下：<font color=cyan>在设置中搜索 external tools， 打开后，添加，主要是把路径设置好即可。这样在工程主目录右键即可看到 external tools 里的 qt designer (添加时自己设置的名字)</font>。

最后，安装打包程序 pyinstaller:

```python
pip install pyinstaller
```

# 开发流程

首先，使用 Qt Designer 设计需要的图形界面，然后保存，如 `test.ui`.

其次，进入该文件的目录，打开 shell 或 cmd，输入如下命令，把其转化为 .py 文件:

```bash
pyuic5 -x test.ui -o main.py
```

只需要把它添加到 PyCharm 中即可，方法如下：<font color=cyan>在设置中搜索 external tools， 打开后，添加，program 选择程序路径，arguments 填写 `$FileName$ -o $FileNameWithoutExtension$.py`,  working directory 填写 `$FileDir$`， 设置好点确定即可。这样在工程主目录右键即可看到 external tools 里的 PyUIC5 (添加时自己设置的名字)</font>。

然后，在该文件中开发，增加各按钮等的功能函数。此时，运行程序能够看到程序演示效果。

最后，使用 pyinstaller 将 Python 程序打包成可独立运行的应用程序。因为其将运行该程序的所有依赖环境都打包进一个 .exe 或 .app 文件中，所以可以拷贝或传播到其他电脑上使用。打包方法如下（首先进入工程目录，假设 main.py 是程序主入口）：

```bash
# 常用方法，打包成一个文件夹，该方法比加 -F 打包成一个文件的程序启动速度快3-6倍
pyinstaller main.py

# -w (--noconsole) 设置程序运行时不显示黑框；-i icon.ico 设置程序图标
pyinstaller -w -i icon.ico main.py

# 添加额外数据，如图像资源
# pyinstaller x.py --add-data="源地址;目标地址"。 windows以;分割，linux以:分割
# 如下，会把 main.py 同目录下的 resources 文件夹，打包到 dist/main 文件夹中，命名仍然是 resources
pyinstaller main.py --add-data resources;resources

# 个人常用方法
# 不显示控制台，清空上次打包痕迹，设置图标，打包到release1.0，添加资源
pyinstaller main.py -w --clean -i icon.ico --distpath release1.0 --add-data resources;resources
```

详细打包方法可参考我的另一篇：[Python 程序编译成 exe 可执行文件](https://xujinzh.github.io/2020/07/21/python-to-exe/).

此时，点击 dist 目录下的 main.exe 或 main.app 就可以运行程序了。

<font color=cyan>注意：打包时建议使用 conda 另外创建一个新的虚拟环境，该坏境包含运行程序必须的包即可，其他建议不要安装，这样打包的程序会较小。</font>

利用 conda 创建虚拟环境的方法可参考我的另一篇：[Conda 配置其他虚拟环境 jupyter kernel](https://xujinzh.github.io/2020/03/25/conda-jupyter-env-kernel/).
