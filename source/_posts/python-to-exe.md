---
title: Python 程序编译成 exe 可执行文件		
author: Jinzhong Xu
mathjax: true
date: 2020-07-21 17:56:21
tags:
- python
- pyinstaller
categories: 
- technology
- python
---

Python 是一种解释型语言，编写的程序只能通过解释器来执行，对于一些想要隐藏源代码或者只想在 Windows, Linux, Mac 上通过鼠标双击直接运行程序的同学，直接给 .py 文件不是一个好的解决方法，下面解释两个软件来方便把python 文件转化为 exe 等程序。

<!--more-->

# pyinstaller

pyinstaller 可以在 Windows, Linux, Mac 等平台上使用。

安装

```bash
pip install pyinstaller
```

使用（进入主程序文件夹）

```bash
cd C:\Users\xujin\Downloads\Tetris

# 打包程序，tetris.py 为整个工程的程序入口，类似于 main
# 打包成一个文件夹，包含很多文件和文件夹；-D, --onedir 为默认选项
pyinstaller tetris.py
pyinstaller tetris.py -D 

# 默认打包到 dist 目录下，可使用如下方法改变目录名为 DIR
pyinstaller tetris.py --distpath DIR

# 清空上次打包时产生的文件，重新打包
pyinstaller tetris.py --clean

# 打包成一个文件
pyinstaller -F tetris.py

# 打包成一个文件, 并且不显示黑控制台
pyinstaller --onefile -w tetris.py
# 或者如下简短命令
pyinstaller -F -w tetris.py

# 打包时添加图标
pyinstaller -F -w -i icon.ico tetris.py

# 个人常用
pyinstaller tetris.py -i icon.ico
```

在 dist 中有编译好的 tetris.exe 文件。这时候，可以删除 build 文件夹，把 dist 里面的 tetris.exe 移动主目录下，删除 dist ，删除 tetris.pym 文件。运行 tetris.exe 就可以了。

<font color=cyan>注意：打包时建议使用 conda 另外创建一个新的虚拟环境，该坏境包含运行程序必须的包即可，其他建议不要安装，这样打包的程序会较小。</font>

利用 conda 创建虚拟环境的方法可参考我的另一篇：[Conda 配置其他虚拟环境 jupyter kernel](https://xujinzh.github.io/2020/03/25/conda-jupyter-env-kernel/).

当打包 PyQt5 编写的 Python 图形程序时，如主窗口有图片 (background.jpg)， 建议使用相对路径(如，./resources/assets/background.jpg, icon.ico)，然后使用 pyinstaller 打包后，将整个 resources 目录拷贝到 dist 里面，这样与 .exe 文件又形成了正确的相对路径，然后使用 NSIS & HM NIS EDIT 打包安装程序时，将 HM NIS EDIT 里的第5步设置为主目录 dist，这些就会将整个 resources 文件和 .exe 都按照原位置安装。

# auto-py-to-exe

安装

```bash
pip install auto-py-to-exe
```

使用

```bash
auto-py-to-exe
```

其实，auto-py-to-exe 也是调用 pyinstaller 来编译的，只是提供了一个建议的 GUI 界面，编译的结果在主目录下的 output 文件夹下，类似 dist ，可以把 tetris.exe 移动到上一层文件夹下，即主目录下，删除 output，运行 tetris.exe 就可以了。

# NSIS

这个 NSIS 不是编译 .py 文件撑 exe 可执行文件的，而是把上面的结果打包成一个可在 Windows 上的安装包程序的（即使用其打包的文件可进行程序安装），方法如下：

1. 下载安装 [NSIS](https://nsis.sourceforge.io/Download)；
2. 运行 NSIS；
3. 打包 Tetris 主目录成 Tetris.zip；
4. 在 NSIS 中点击 Compiler 下的 Installer based on .ZIP file；
5. 检索完毕后，选择右下角的 generate；
6. generate 结束后，可以选择 test 进行测试，或者直接关闭；
7. 双击 Tetris.exe 进行安装软件。

# HM NIS EDIT

在打包安装包程序时，仅仅使用 NSIS 是不够方便的，比如，不能自己设置 ico 图标、打包的安装包程序安装后没有卸载方法。使用 HM NIS EDIT 可以方便解决这些问题，但需要事先安装好 NSIS，然后安装 HM NIS EDIT，并进行打包，方法如下：

1. 下载安装 [HM NIS EDIT](http://hmne.sourceforge.net/index.php)；
2. 点击文件 --> 新建脚本：向导，安装个人需要进行想要设置，如程序名、版本号、出版人等；一次点击下一步；
3. 在第3步时，设置安装程序图标和语言；
4. 在第4步时，设置授权文件，根据个人需要，没有可以删除，留空；
5. 在第5步时，比较关键，分别把 pyinstaller 打包的 EXE 程序添加进去，如果有图片资源等，需要添加 AddDir Tree，选择主目录（相对于 EXE 程序的，即 EXE 所在目录）；
6. 最后，保持脚本，如果认为没有问题，可以点编译脚本，建议先不点，打开文件后查看是否都正确，然后再右键编译，此时，就会生成 Setup.exe 安装包程序。

# 参考链接

1. [别再问我怎么Python打包成exe了！](https://zhuanlan.zhihu.com/p/162237978)
1. [详细介绍：使用NSIS和VNISEdit制作一个自解压的exe安装包](https://www.codenong.com/cs105537269/)
1. [打包之后找不到图标\_使用NSIS打包程序_第四张牌的博客-程序员宅基地](https://www.cxyzjd.com/article/weixin_36356040/112618193)
1. [打包之后找不到图标_使用NSIS打包程序](https://blog.csdn.net/weixin_36356040/article/details/112618193)
1. [[Python] PyInstaller 製作可夾帶圖片的可執行檔](https://clay-atlas.com/blog/2019/11/13/python-chinese-tutorial-packages-pyinstaller-image-exe/)

