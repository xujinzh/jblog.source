---
title: Linux 批处理：修改文件名和文件内容
mathjax: true
author: Jinzhong Xu
date: 2021-02-09 17:47:50
tags:
- rename
- sed
- linux
categories:
- technology
- linux
---

在 Linux 系统上，包含有大量相同字段的文件名，想要把这些字段（中文字段）改成其他字段（数字或其他英文字段），因为某些程序的需要，不支持中文文件名，此时可以使用命令 rename 批量修改文件名。当文件中包含有大量相同字段，想要修改，但是，收到修改又比较浪费时间。此时，可以使用命令 sed 完成。

<!--more-->

# rename

在 linux 系统中重命名文件，经常用到 mv命令，批量重命名文件 rename 是最好的选择。

linux的 rename命令有两个版本，一个是c语言版本的，一个是perl语言版本的，判断方法：输入man rename 

## C语言版本

```html
rename 原字符串 新字符串 文件名
```

示例：

```bash
rename 第三阶段 3 *
# * 代表所有字符
# ？代表单个字符
```

## Perl语言版本

```html
rename 's/原字符串/新字符串/' 文件名
```

示例：

```bash
rename 's/第三阶段/3/' *
rename "s/$/.txt/" * //把所有的文件名都以txt结尾
```

# sed

linux下批量替换文件内容

```bash
# 针对单个文件
sed -i "s/查找字段/替换字段/g" 文件名
# 针对目录下的所有文件
sed -i "s/查找字段/替换字段/g" `grep 查找字段 -rl 路径`
# r 表示递归
# l 表示 -files-with-matches
```

示例：

```bash
sed -i "s/www.baidu.com/baidu.com/g" file.config
sed -i "s/www.baidu.com/baidu.com/g" `grep www.baidu.com -rl /home`
```



# 参考链接

1. [linux下rename命令用法详解(重命名文件)](https://blog.51cto.com/jiemian/1846951)

2. [rename](https://wangchujiang.com/linux-command/c/rename.html)

3. [每天学习一个命令: rename 批量修改文件名](http://einverne.github.io/post/2018/01/rename-files-batch.html)

4. [linux下批量替换文件内容](https://blog.csdn.net/hnlyyk/article/details/49299909)

   

   

