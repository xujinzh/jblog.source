---
title: cmake make gcc 的关系
mathjax: true
author: Jinzhong Xu
date: 2021-02-03 11:27:26
tags:
- cmake
- make
- gcc
- c
categories:
- technology
- c/c++
---

在运行 C 程序时，我们都知道需要通过 gcc(GNU Compiler Collection，可以编译C、C++、Objective-C、Fortran、Java等编写的源代码) 将源代码编译为二进制文件，才能够执行。

<!--more-->

# gcc

简单代码使用 gcc 可直接编译。具体过程如下：

```bash
## 创建源文件
vim hello.c
```

编写源代码：

```c
// 写入如下C代码
#include<stdio.h>

int main() {
    printf("hello 20210203 \n");
    return 0;
}
```

编译源代码为机器码：

```bash
# 使用gcc编译
gcc hello.c -o hello
# 也可以将-o和hello写在一起
gcc hello.c -ohello
# 如果有多个源文件，可以这样编译
gcc hello1.c hello2.c -o hello
# 运行的结果
./hello
```

这是一个小程序，可以使用该方法编译。但是，当在大型程序开发时，这种方法就显得笨拙。这时常使用 make 和 cmake 来自动进行编译。

# make

make 基于 makefile，其中 makefile 中包含了 gcc 进行编译的各种命令，make 通过 makefile 进行批处理。

对于相对简单的 makefile 可以使用人工进行书写，但是，对于复杂的工程，makefile 也将会变得很复杂，这时就需要一个自动创建 makefile 的工具，那就是 cmake。

# cmake

cmake 可以创建 makefile，方便 make 进行编译。但是 cmake 需要 CMakeLists.txt 才能够生成 makefile. 而一般 CMakeLists.txt 是非常简单的。

需要知道的是，出了 cmake，还有autotools，qmake等可生成 makefile。

# 参考链接

1. [gcc,make,cmake傻傻分不清楚？](https://blog.csdn.net/libaineu2004/article/details/77119908)

   