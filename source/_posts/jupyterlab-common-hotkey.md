---
title: JupyterLab 的常用快捷键
mathjax: true
author: Jinzhong Xu
date: 2021-04-07 08:58:14
tags:
- jupyterlab
- hotkey
- python
- markdown
categories:
- technology
- python
---

JupyterLab 是一款强大的科学生成工具，作为一种基于 WEB 的集成开发环境，可以方便的编写 Python、R、C、Java 等等代码，并且可以操作终端，查看文档、图片等，最优秀的是可以编辑 markdown 文本，这样在代码上下文中书写 markdown 文档。而 Notebook 是我们最常使用的，下面介绍一些常用的 JupyterLab 中 Notebook 快捷键，方便快速编写代码和书写 markdown 文档。

<!--more-->

# 模式介绍

在 JupyterLab 的 Notebook 中有两种模式，分别是编码模式（Edit mode）和命令模式（Comman mode）。编码模式就是光标在 code 栏中闪烁时的模式，而命令模式就是点击 code 栏中括号后，code 栏变成灰色时的模式。

```markdown
在编码模式下可以通过按 Esc 键进入命令模式
```

```
在命令模式下可以通过按 Enter 键进入编码模式
```

# 运行代码

这里介绍运行一个 code 栏代码的方法。

```
Ctrl + Enter	运行本栏代码，保持在本栏并进入命令模式
```

```
Shift + Enter	运行本栏代码，跳到下一栏并进入命令模式
```

```
Alt + Enter	运行本栏代码，跳到下一栏并进入编辑模式
```

# 增加代码栏

在编辑时往往需要插入新的代码栏，快捷键都是在命令模式下进行的

```
a	在本栏代码前增加一栏，并跳到新增加的一栏，仍处在命令模式下
```

```
b	在本栏代码后增加一栏，并跳到新增加的一栏，仍处在命令模式下
```

# 删除不需要的代码栏

在编辑时，有些无用的代码栏或空白栏需要删除，首先进入命令模型下，通过上下键跳到需要删除的代码栏

```
dd	删除本代码栏，并自动跳到下一栏代码栏，仍处在命令模式下
```

# 代码栏 Code 和 Markdown 切换

当新增一栏后，默认是 Code 模式，如何切换到 Markdown 模式，可以使用如下快捷键。快捷键都是在命令模式下进行

```
m	切换到 Markdown 模式，仍处在命令模式下，按下 Enter 可进入编辑模式
```

```
y	切换到 Code 模型，仍处在命令行模式下，按下 Enter 可进入编辑模式
```

# 参考链接

1. [JupyterLab，极其强大的下一代notebook！](https://zhuanlan.zhihu.com/p/87403131)
2. [jupyter code和markdown转换](https://blog.csdn.net/qq_35423500/article/details/79565146) 