---
title: 如何进行目标跟踪
author: Jinzhong Xu
mathjax: true
date: 2020-08-24 16:30:19
tags:
- vot
- cv
categories:
- research
- object tracking
---

前一篇我们介绍了目标跟踪的基本原理是定义目标、生成候选框、提取特征、匹配最优候选框。根据这一原理，本篇介绍如何具体进行目标跟踪。

从具体的流程上来说，一次实现过程如下

输入图像（前一帧）---> **候选框生成模型** ---> **特征提取模型** ---> **最优候选框模型** ---> 输出带有预测候选框的图像（后续帧）

<!--more-->

# 候选框生成模型

从前一帧到后续帧，目标可能出现位置变化、尺寸变化、旋转、光照变化等，因此需要对这些变化进行建模表示，常用的方法有概率采样方法、滑窗方法、循环移位方法等

## 概率采样方法

通过放射变换得到候选框。假设前一帧的矩形框的为$x$，仿射变换的系数矩阵为$A$，后续帧的候选框为$y$，则
$$
y=Ax
$$
其中，仿射变换矩阵$A$描述了目标的位置变换、尺寸变换、旋转变换、长宽比变换等信息。

概率采样表示放射变换矩阵$A$中的各个参数符合某种概率分别（如高斯分布）的随机变量，并生成不同数量的候选框。

## 滑窗方法

滑窗方法模拟目标在视频中的移动过程。以某个形状和大小的结构元素（称为窗）在前一帧中按一定的空间间隔移动，每次移动后覆盖的图像中的相应像素即为后续帧的候选框。该方法只能表示位置变换，其他如尺寸变换、旋转变换还需进一步处理。

## 循环移位方法

本质上来说，循环移位方法是滑窗方法的一种。但其使用较广，所以单独列出。将前一帧矩形框中的像素按照某种排列组成一个向量$a$，以向量$a$为基准向量生成循环矩阵。其中循环矩阵的每一行都对应一个候选框，求得该循环矩阵得最大特征值认为是最优的矩形框。该方法生成的候选框仅仅具有位置变换（如旋转变换），其他变换需要额外处理。因利用快速傅里叶变换求解循环矩阵的特征值比较快速，因此利用该生成模型的相关滤波算法速度较快，即具有较高的FPS.

# 特征提取模型

得到候选框后，还需要进一步的从候选框中提取图像特征。常用的特征包括颜色特征、空间特征、形状特征、纹理特征、深度卷积特征等。特征越“深”（抽象但不直观，深度特征），对目标判别能力好，特征越“浅”（具体且直观，如颜色特征），对目标的空间位置信息保留越好。想好了需要哪些图像特征后，就需要把这些特征表示成计算机能够理解和计算的数值量，常用的方法包括朴素方法（naive，像素值）、统计方法（statistics，直方图）、数学变换（transform，像素值梯度等）

特征提取模型就是特征和提取方法的结合。概括起来就是

特征包括颜色、形状、纹理等，颜色包括灰度、彩色等

提取方法包括朴素方法、统计方法、变换方法等，朴素方法如像素值，统计方法如直方图，变换方法如梯度等

## 特征模型的分类

特征模型按大类别分为手工提取特征（Hand-Crafted）和深度学习提取的深度特征（Deep-Learning，如CNN）。

其中，手工提取特征方法包括Naive、Histogram series、Haar-lik等，Naive包括Gray-scale、Color，Histogram series包括Histogram of colors (HoC)、Histogram of gradients（HoG)，Color包括RGB、HSV、Color Names(CN)、LAB等。

# 最优候选框模型

在目标跟踪中，如何从候选框中选择最优的矩形框作为后续帧的目标预测是非常重要的和主要问题，直观上，就是从后续帧中找到与前一帧最“像”的目标，但这个“像”可以有多种定义方法。

在计算机视觉中，该问题就是一个匹配问题，是目标跟踪的核心问题，它直接影响着算法的性能。及时前两部差强人意，但是优秀的匹配算法能够在一定程度上弥补它们的不足。

匹配就是一个相似性度量的问题。比较的对象是前一帧的目标矩形框（常称为ground truth）和后续帧的各候选框（bounding-box）。可以将它们之间的相似度用距离来衡量，这里的距离是抽象距离，如空间距离（两个矩形框之间像素的距离，如 Minkowski distance $l^p$、Manhattan distance $l^1$、Euclidean distance $l^2$、Chebyshev distance $l^{\infty}$），可以是两个概率分布的距离（矩形框服从的概率分布），如Kullback–Leibler (KL) 散度，Bhattacharyya distance， 交叉熵，Wasserstein distance 等。

该模型主流的匹配方法有两种，分别是生成式方法（generative）和判别式方法（discriminative），主要区别就在于是否有背景信息的引入，生成式方法把跟踪问题建模成拟合或多分类问题，而判别式方法把跟踪问题定义为二分类问题。

一般在几帧匹配算法后会引入更新模块，这是因为，匹配算法得到了一系列的参数，应用这些参数即可对当前帧的目标位置进行预测。如果在后续所有帧的预测过程中都应用这些参数，可能会出现的结果是预测趋向不准确，最终导致跟踪的失败。其可能的原因包括累积误差、外因（如遮挡、光照变化）、以及内因（如目标外观变化、快速运动）等。如果引入更新模块，在每若干帧之后根据之前的预测结果更新匹配算法的参数，则可以减小误差，提高跟踪的准确性。

但是，并不是每一帧都要进行更新模块，因为这样会消耗更多时间，导致跟踪的FPS较低。研究者更多的是采用多步一更新的策略。

# 参考链接

[视觉目标跟踪漫谈：从原理到应用](https://developer.aliyun.com/article/766769) 