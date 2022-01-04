---
title: 目标跟踪算法的分类
author: Jinzhong Xu
mathjax: true
date: 2020-08-24 17:50:46
tags:
- vot
- cv
categories:
- research
- object tracking
---

目标跟踪算法按照不同的分类方法可以分为很多类别，如按提取的特征分类、特征提取方法分类、最优候选框匹配模型分类等。研究者大多是按照最优候选框匹配模型分类，这里也按此给出目标跟踪的算法分类。

按此方法，可以分为生成式算法和判别式算法。前者较老、后者较优。

<!--more-->

# 生成式算法

核心思想是衡量前一帧的矩形框与后续帧的相似度，选择最相似的候选框作为跟踪结果。常按照相似性度量的选择细分为三类：空间距离、概率分布距离、综合方法。

## 空间距离

利用最优化理论将跟踪问题转化为空间距离最小化问题。如 IVT(Incremental Learning for Visual Tracking) 和 ASLS(Visual tracking via adaptive structural local sparse appearance model). 其算法的核心思想是：计算当前帧候选框的像素灰度值与上一帧预测目标的像素灰度值之间的Euclidean distance，然后取距离最小的候选框作为当前帧的预测目标。在特征提取时应用了奇异值分解等技术来减小计算复杂度。

## 概率分布距离

利用最优化理论将跟踪问题转换成概率分布距离最小化问题。如 CBP (Color-Based Probabilistic) 和 FRAG (robust FRAGments-based) 。其算法的核心思想是：计算当前帧候选框的颜色直方图分布与上一帧预测目标的颜色直方图分布之间的 Bhattacharyya distance，然后取距离最小的候选框作为当前帧的预测目标。

## 综合方法

隐式的计算连个矩形框的距离。如 Mean Shift 算法和 Cam Shift 算法。借鉴了机器学习中 mean shift 聚类算法的思想，在每一帧中利用上一帧预测目标的颜色直方图分布，计算该帧中相应位置的像素的颜色直方图分布，然后进行聚类得到其分布的均值，其对应的像素位置是该帧中预测目标的中心位置，然后加上候选框宽高等信息即可得到当前帧预测目标的空间位置。在 Mean Shift 算法中，宽高信息是固定的，因此其无法应对目标尺度和旋转变化，而 Cam Shift 通过将图像矩引入相似度匹配，得到目标尺度和旋转信息，进一步提高了算法的性能。

# 判别式算法

判别式方法侧重于将目标视作前景，然后将其从其它被视作背景的内容中分离出来。判别式方法应用了分类算法的思想，将跟踪问题转换成二分类问题。包括经典机器学习算法、相关滤波算法、深度学习算法。

## 经典机器学习算法

STRUCK (STRUCtured output tracking with Kernels) 和 Tracking-Learning-Detection (TLD) 。STRUCK 和 TLD 算法分别采用经典机器学习算法中的支持向量机 (support vector machine) 和集成学习 (ensemble learning) 进行分类，并采取了一系列优化方法来提高算法的性能。

## 相关滤波算法

方法来源于通信理论，用一个模板与输入进行相关操作，通过得到的响应（输出）来判断该输入与模板的相似程度，即相关性。如 KCF和ECO.

## 深度学习算法

利用CNN提取单帧的深度卷积特征。利用LSTM提取前后帧信息。如 SiamRPN 和 MDNet.

# 目标跟踪算法分类

![avatar](https://raw.githubusercontent.com/foolwood/benchmark_results/master/img/recent_Tracker_development.png)

# 参考链接

[视觉目标跟踪漫谈：从原理到应用](https://developer.aliyun.com/article/766769)

