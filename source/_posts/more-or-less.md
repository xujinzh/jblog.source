---
title: Linux命令 more 和 less
author: Jinzhong Xu
date: 2020-03-13 15:43:08
tags:
- linux
categories: technology
---

Linux系统有命令more和less来在终端查看文件内容，但是，两者有什么区别呢？总的来说，less比more更有效率更快，因为less不会立即加载整个文件，而more命令会一次加载整个文件内容。

<!--more-->

# more命令

```bash
more test.log
cat test.log | more
```

查看test.log文件内容，同时显示剩余内容百分比，可以使用 **Enter**（一次一行）和**Spacebar**（一次一页）来翻页，使用**q**退出查看。此时，文件内容会保留在终端上。

```bash
more -5 test.log
```

只显示文件test.log的前5行

```bash
more +6 test.log
```

从文件test.log的第6行开始显示，内容铺满整个屏幕，具体看屏幕大小

# less命令

```bash
less test.log
cat test.log | less
```

查看test.log文件内容，可以使用**Enter**（一次一行）和**Spacebar**（一次一页）来翻页，另外，也可以使用**pageup**和**pagedown**来上翻页和下翻页。使用**q**退出查看。此时，文件内容不会保留在终端上，而是退回命令行。

```bash
less +5 test.log
```

从文件test.log的第5行开始显示

```bash
less -N test.log
```

显示文件内容的同时显示行号

```bash
less -e test.log
less -E test.log
```

当查看到最后页面时自动退出，不需要再按**q**键

```bash
less +/ssh test.log
```

显示第一次出现ssh的地方在最顶部

# 参考资料

- [Learn Why ‘less’ is Faster Than ‘more’ Command for Effective File Navigation](https://www.tecmint.com/linux-more-command-and-less-command-examples/)