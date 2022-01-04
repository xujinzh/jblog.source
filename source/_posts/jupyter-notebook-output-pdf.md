---
title: 把 jupyter notebook 导出为 pdf
mathjax: true
author: Jinzhong Xu
date: 2021-05-25 17:54:36
tags:
- jupyter
- notebook
- pdf
categories:
- technology
- python
---

在 Jupyter Notebook 中书写代码非常方便，同时可以书写 Markdown 说明文档，使得其展示、交流非常高效。但是，常常需要我们把 Jupyter Notebook 导出为 PDF 才更容易交流，这样接收者只需要能打开 PDF 即可，不需要专门安装打开 .ipynb 的工具。但是，从Jupyter Notebook 导出 PDF 需要安装一些包，这里以 Debian 平台为例。

<!--more-->

```shell
sudo apt update
sudo apt install texlive texlive-latex-extra pandoc texlive-xetex
```

