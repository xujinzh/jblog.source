---
title: 把 EXCEL 表格转换为 LATEX 代码
mathjax: true
author: Jinzhong Xu
date: 2021-09-30 12:03:20
tags:
- latex
- excel
categories:
- research
- article
---

使用 $\LaTeX$ 书写论文被越来越多的人采用，其主要的优势是让写作者不用在格式上花费太多的时间，把主要时间用在文章的内容写作上。我们平常处理的数据大多会以 EXCEL 表格的形式保存，那么如何将 EXCEL 表格中的数据快速复制到 $\LaTeX$ 源文件中呢，下面介绍一种工具，它能够非常快捷准确的将表格数据转化为  $\LaTeX$ 格式，并利用  $\LaTeX$ 中的包快速编译出表格信息。本篇默认 EXCEL 和 $\LaTeX$ 编译器已经安装且可使用。

<!--more-->

# 下载 Excel2LATEX

在 CTAN 官网上可以下载最新版的 Excel2LATEX: https://ctan.org/pkg/excel2latex.

# 安装

解压缩后，将文件夹放在一个自己喜欢的目录下，放好后该文件夹不应该移动或删除，点击文件：Excel2LaTeX.xla 并启用即可使用它。

如果想让 EXCEL 默认加载该程序，可以在 EXCEL 程序中点击文件---选项---加载项---转到---浏览（选择文件 Excel2LaTeX.xla）---方框内打勾---确定。

# 使用

利用 EXCEL 打开一个表格，选择表格内容，点击加载项，选择 convert table to latex，点击 copy to clipboard，将其粘贴到  $\LaTeX$ 源文件中。

在  $\LaTeX$ 源文件中头部添加包：

```latex
\usepackage{booktabs}
# 处理对齐问题：\toprule， \midrule
```

编译即可。

# 参考链接

1. [Excel2LATEX – Convert Excel spreadsheets to LATEX tables](https://ctan.org/pkg/excel2latex)

   
