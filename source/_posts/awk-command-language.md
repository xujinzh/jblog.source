---
title: Linux 中的 awk 命令
mathjax: true
author: Jinzhong Xu
date: 2021-09-06 09:40:36
tags:
- awk
- linux
categories:
- technology
- linux
---

awk 是一个处理文本的应用程序，也是一门语言。基本上所有的 Linux 发行版都自带该程序。它依次处理文件的每一行，并读取里面的每一个字段，尤其是日志文件、CSV文件等。本篇介绍 awk 如何在命令行上使用，以 Debian 10.8 为例。

<!--more-->

# 基本用法

```bash
# 模式或条件可省略
awk '模式 {动作}' 文件名

# 例子 1
awk 'NR < 3 {print $0}' ~/.bashrc

# 例子 2
echo '21 25 26' | awk '{print sin($1), cos($NF)}'
0.836656 0.646919

# 例子 3
echo '21:25:26' | awk -F ':' '{print sin($1), cos($NF)}'
0.836656 0.646919

# 例子 4
echo '21:25:26' | awk -F ':' '{print $1 * $NF}'
```

上例 1 中，`NR` 是一个变量表示行号，`NR < 3` 表示只取前 2 行，为模式或条件；大括号中的为动作，`print $0` 表示打印所有列；`~/.bashrc` 表示文件名。因此，命令是将文件 `~/.bashrc` 中前 2 行所有列打印出来。

上例 2 中，通过管道符 `|`将 `echo` 的结果传给 `awk`，字符串 `21 25 26`以空格为分隔符，`awk`中默认的列分隔符就是空格，`$1` 表示第1列 `21`，依次类推，`NF`是变量表示最后一列，这里是 `26`，函数 `sin`, `cos` 是 awk 中自带的函数。两个函数之间的`,`表示打印时以空格为分隔符。

上例 3 中，与例 2 不同的是，字符串`21:25:26`的分隔符是 `:`，在 awk 中以参数 `-F` 指定分隔符。

上例 4 中，给出了自定义运算输出结果。

# 变量

除了上面例子中介绍的内置变量`NR, NF` 外，还有很多变量，这里列出常用的几个变量。

```markdown
FILENAME：当前文件名
FS：字段分隔符，默认是空格和制表符。
RS：行分隔符，用于分割每一行，默认是换行符。
OFS：输出字段的分隔符，用于打印时分隔字段，默认为空格。
ORS：输出记录的分隔符，用于打印时分隔记录，默认为换行符。
OFMT：数字输出的格式，默认为％.6g。
```

```bash
# 如打印时添加行号
awk 'NR < 3 {print NR ")", FILENAME}' ~/.bashrc
1) /home/jinzhongxu/.bashrc
2) /home/jinzhongxu/.bashrc
```

# 函数

除了第一节例子中介绍的内置函数`sin, cos`外，还有很多函数，这里列出常用的几个函数。

```markdown
toupper(x): 字符串大写
tolower(x)：字符串小写。
length(x)：返回字符串长度。
substr(x)：返回子字符串。
atan2(y,x)：Arctan of y/x between -pi and pi.
exp(x)：Exponential function.
int(x)：Returns x truncated towards zero.
log(x)：Natural logarithm.
sqrt()：平方根。
rand()：随机数。
```

更多请使用命令 `man awk`查看。

# 条件

条件就是第一节中介绍的模式，可处理更加特定的输出，如

```bash
# 只打印偶数行
awk 'NR % 2 == 0 {print $0}' ~/.bashrc

# 只打印行号大于 3 的行
awk 'NR > 3 {print $0}' ~/.bashrc

# 只打印第一个字符串是 export 的行
awk '$1 == "export" {print $0}' ~/.bashrc

# 只打印第一个字符串是 export 或 alias 的行
awk '$1 == "export" || $1 == "alias" {print $0}' ~/.bashrc
```

# if 语句

if 语句能够处理更复杂的逻辑，如

```bash
# 只打印第一个字符串是 export 的行
awk '{if ($1 == "export") print $0}' ~/.bashrc

# 对于以字符串 export 开头的行直接打印，其他行打印成 "*" 号
awk '{if ($1 == "export") print $0; else print "*"}' ~/.bashrc
```

# 参考链接

1. [awk 入门教程](https://www.ruanyifeng.com/blog/2018/11/awk.html)
2. [Why you should learn just a little Awk](https://gregable.com/2010/09/why-you-should-know-just-little-awk.html)
3. [30 Examples for Awk Command in Text Processing](https://likegeeks.com/awk-command/)

