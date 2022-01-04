---
title: 矢量图和位图
date: 2019-12-20 20:32:32
tags:
- image
- cv
categories: technology
---

矢量图是由线段或曲线来描述图像，同时包含有色彩和位置信息;

位图是由像素点来描述图像，也称为点阵图;

<!--more-->



## 两者有什么特点：

矢量图：可以无限放大或缩小，而不失真，一般体积或占用空间比较小；但，色彩丰富度不如位图；

位图：色彩丰富，但放大后容易不清晰，一般体积比较大

## 两者常见的格式：

矢量图： AdobeIllustrator的 *.AI、*.EPS和SVG、EMF、AutoCAD的 *.dwg和dxf、Corel DRAW的 *.cdr等；

位图： *.bmp、*.pcx、*.gif、*.jpg、*.tif、*.png、photoshop的 *.psd等

## 两者常见使用场景：

矢量图：常用于图标、logo、标识等（ SVG适合于浏览器，EPS适合于LaTeX，EMF才适合Word ）

位图：常用于打印或论文印刷（tif格式，虽体积大但基本不损失信息）、网络传播（jpeg格式和gif格式，压缩比高，体积小但损失信息，可用于一般资料保存；png格式，能够透明或半透明图像，比JPEG大但不适宜印刷）

矢量图可以轻松转化为位图，但是位图转化为矢量图需要大量的计算（可通过网站<a href="https://zh.vectormagic.com/" target="_blank" rel="noreferrer noopener" aria-label="Vector Magic（在新窗口打开）">Vector Magic</a> 转换)

<p>参考自<a href="https://zhuanlan.zhihu.com/p/52047447" target="_blank" rel="noreferrer noopener" aria-label="位图与矢量图的区别（在新窗口打开）">位图与矢量图的区别

