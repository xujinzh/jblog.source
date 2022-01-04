---
title: 三维深度学习简单介绍
date: 2019.12.21
tags:
- 3d
- dl
categories: research
---

文章《<a rel="noreferrer noopener" aria-label="超越像素平面：聚焦3D深度学习的现在和未来（在新窗口打开）" href="https://www.jiqizhixin.com/articles/091203" target="_blank">超越像素平面：聚焦 3D 深度学习的现在和未来</a>》（英文版对应《<a rel="noreferrer noopener" aria-label="Beyond the pixel plane: sensing and learning in 3D（在新窗口打开）" href="https://thegradient.pub/beyond-the-pixel-plane-sensing-and-learning-in-3d/" target="_blank">Beyond the pixel plane: sensing and learning in 3D</a>》）中介绍了三维数据的获取，表示，以及三维深度学习的进展。

<!--more-->

三维数据的获取可以通过

1. 立体视觉系统（多目相机）
2. RGB-D（结构光和 TOF）

三维数据的表示形式包含

1. 点云数据
2. 体素网格点
3. 多边形网格
4. 多视角表示

三维深度学习最新研究趋势

- 以基于点云数据为研究对象的 PointNet 为开端，流行的方法是结合点云数据和多视角的二维图像数据作为深度学习网络的输入