---
title: 同步与异步
author: Jinzhong Xu
mathjax: true
date: 2020-03-18 16:49:38
tags:
- computer
- compile
categories: technology
---

Synchronous（同步）和Asynchronous（异步）是编程时比较重要的一个概念。而且，同步与异步的概念与我们日常生活中的含义不同，容易导致误解。这里给出计算机科学上两者的解释。

<!--more-->

# 同步

同步类似于实时打电话。什么意思，就是必须同时在线。用计算机术语就是单线程模式，函数或方法调用后，必须等待直接结束，等到返回值后才能释放资源。

同步属于阻塞模式。

一个实际的例子是，用户登录，需要对用户验证完成后才能登录系统。

效率低。

# 异步

异步类似于分时发短信。就是可以不用同时在线，有空闲时再处理。用计算机术语就是多线程模式，函数或方法调用后，无需等待，可以去执行其他任务。

异步属于非阻塞模式。



一个实际的例子是，页面数据加载过程，不需要等所有数据获取后再显示页面。

效率高。

**参考资料：**

1. [同步和异步的区别](https://blog.csdn.net/tennysonsky/article/details/45111623)
2. [计算机领域中的同步（Synchronous）和异步（Asynchronous）](https://blog.csdn.net/shiyong1949/article/details/80854656)
3. [简述同步和异步的区别](https://www.jianshu.com/p/ee2dcd1ccfc6)