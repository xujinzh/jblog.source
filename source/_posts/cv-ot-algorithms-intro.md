---
title: 视觉目标跟踪算法简介
mathjax: true
author: Jinzhong Xu
date: 2021-08-20 12:55:24
tags:
- cv
- vot
- ml
categories:
- research
- object tracking
---

单目标跟踪是指给定第一帧目标框的情况下，在视频的后续帧中自动地给出目标的位置和大小。难点在于复杂场景下目标姿态变化、环境光照变化、尺寸变化、背景干扰、遮挡等，以及实际应用中需要满足实时性。

<!--more-->

# 算法分类

目标跟踪算法的发展大致分成两个阶段，一个是2012年以前，一个是2012年以后。以深度学习方法的引入为拐点。

目标跟踪算法的种类大致分为两类，一类是生成式（**generative**）算法，一类是判别式（**discriminative**）算法，目前判别式算法是主流算法。

目标跟踪算法的研究大致分为两个方向，一个是相关滤波方向，一个是深度学习方法，基于相关滤波的跟踪算法因利用快速傅里叶变换，处理速度较快，基于深度学习的跟踪算法因能自动提取目标图像更强大的特征，精度较高。

![](https://github.com/xujinzh/benchmark_results/blob/master/img/recent_Tracker_development.png?raw=true)

# 生成式算法

生成式算法采用特征模型描述目标的外观特征，再最小化跟踪目标与候选目标之间的重构误差来确认目标。此方法着重于目标本身的特征提取，忽略目标的背景信息，因而在目标外观发生剧烈变化或者遮挡时，容易出现目标漂移或目标丢失。

生成式目标跟踪算法主要是在2010年以前发展起来的，代表算法有卡尔曼滤波（Kalman Filters）、CAMShift 等。

## 卡尔曼滤波

卡尔曼滤波器用于估计线性系统的状态，其中假设状态服从高斯分布。卡尔曼滤波目标跟踪算法是一种单目标跟踪算法。

Broida 和 Chellappa [[1986-TPAMI-Estimation of Object Motion Parameters from Noisy Images](https://ieeexplore.ieee.org/abstract/document/4767755)] 使用卡尔曼滤波器来跟踪噪声图像中的点。在基于立体相机的对象跟踪中，Beymer 和 Konolige [[1999-ICCV- Real-time tracking of multiple people using continuous detection](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.46.7987&rep=rep1&type=pdf)] 使用卡尔曼滤波器来预测对象在 $x-z$ 维度上的位置和速度。 Rosales 和 Sclaroff [[1999-CVPR-3D trajectory recovery for tracking multiple objects and trajectory guided recognition of actions](https://ieeexplore.ieee.org/abstract/document/784618)] 使用扩展卡尔曼滤波器从 2D 运动估计对象的 3D 轨迹。 KalmanSrc 提供了用于卡尔曼滤波的 Matlab 工具箱。

卡尔曼滤波器的一个限制是假设状态变量呈正态分布（高斯分布），这个限制在粒子滤波（单目标跟踪算法）中得到解决。ParticleFltSrc 提供了使用粒子滤波进行跟踪的 Matlab 工具箱。

## CAMShift

CAMShift (**Continuously Adaptive Mean Shift**) 是 Gary Bradski 于 [[1998-Computer Vision Face Tracking For Use in a Perceptual User Interface](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.14.7673)] 引入的一种基于颜色直方图（colour histogram，常使用HSV颜色命名空间）的目标跟踪方法。 CAMShift 算法（又叫连续自适应均值偏移算法）衍生自均值偏移（mean shift）算法，它负责寻找要跟踪的对象的概率分布的中心。主要区别在于 CAMShift 会根据搜索窗口大小自行调整，例如，当对象大小随着它们靠近或远离相机而发生变化时。

# 判别式算法

判别式算法将目标跟踪看作一个二元分类问题，通过训练关于目标和背景的分类器来从候选场景中确定目标，可以显著区分背景和目标，性能鲁棒，渐渐成为目标跟踪领域的主流方法，目前大多数基于深度学习的目标跟踪算法都属于判别式方法。

判别式目标跟踪算法主要是在2010年以后开始流行，代表算法有 Struck、TLD、相关滤波类算法、深度学习类算法等。

## Struct

Struct 目标跟踪算法是 Sam Hare 等学者于 [[2011-ICCV-Struck: Structured output tracking with kernels](https://ieeexplore.ieee.org/document/6126251)] 引入的一种使用 kernelized structured output support vector machine (SVM) 的自适应 tracking-by-detection、判别式、在线学习的 structured output prediction 目标跟踪算法。Struct 主要提出一种基于结构输出预测的自适应视觉目标跟踪的框架，通过明确引入输出空间满足跟踪功能，能够避免中间分类环节，直接输出跟踪结果。同时，为了保证实时性，该算法还引入了阈值机制，防止跟踪过程中支持向量的过增长。

Struct C++ 源码地址: https://github.com/gnebehay/STRUCK

VOTR (视觉目标跟踪算法库): https://github.com/gnebehay/VOTR

## TLD 

TLD 目标跟踪算法是 Zdenek Kalal 等学者于 [[2010-TPAMI-Tracking-Learning-Detection](https://ieeexplore.ieee.org/abstract/document/6104061)] 引入的一种长时间（long-term）目标跟踪算法，主要包含了三个模块：检测模块、跟踪模块、学习模块。TLD 最适合在跟踪对象被遮挡，不连续出现情况下，进行长时跟踪的应用场合。

TLD Matlab 源码地址：https://github.com/gnebehay/TLD

## 相关滤波

相关滤波类算法最重要的特点是快，都是基于手工设计的特征，最早提出是由 David S. Bolme 在 [2010-MOSSE-Visual object tracking using adaptive correlation filters](https://ieeexplore.ieee.org/abstract/document/5539960) 中提出，基于单通道灰度特征, MOSSE 表示 Minimum Output Sum of Squared Error，达到 669 FPS.

在此基础上，Jo˜ao F. Henriques 在 [2012-ECCV-CSK-Exploiting the Circulant Structure of Tracking-by-Detection with Kernels](https://link.springer.com/chapter/10.1007/978-3-642-33765-9_50) 中提出 CSK，其在 MOSSE 的基础上扩展了密集采样 (加 padding) 和 kernel-trick，达到 362 FPS. 

进一步，João F. Henriques 在 [2015-TPAMI-KCF-High-Speed Tracking with Kernelized Correlation Filters](https://ieeexplore.ieee.org/document/6870486) 中提出 KCF，其在 CSK 的基础上扩展了多通道梯度的 HOG 特征（梯度特征），达到 172 FPS. 

同期，Martin Danelljan 在 [2014-CVPR-CN-Adaptive Color Attributes for Real-Time Visual Tracking](https://openaccess.thecvf.com/content_cvpr_2014/papers/Danelljan_Adaptive_Color_Attributes_2014_CVPR_paper.pdf) 中提出 CN，其在 CSK 的基础上扩展了多通道颜色的 Color Names （颜色特征），达到 152 FPS. 

上面的相关滤波类目标跟踪算法都没有考虑目标尺度的变化，当目标缩小或扩大都会使跟踪结果不理想。因此，针对该问题有学者提出如下改进的相关滤波类目标跟踪算法。

学者 Yang Li 基于 CSK 提出 [2014-ECCV-SAMF-A Scale Adaptive Kernel Correlation Filter Tracker with Feature Integration](https://link.springer.com/chapter/10.1007/978-3-319-16181-5_18)，融合 HOG，CN特征，采用平移滤波器在多尺度缩放的图像块上进行目标检测，取响应最大的那个平移位置及其所在的尺度。

学者 Martin Danelljan 提出 [2014-BMVC-DSST-Accurate Scale Estimation for Robust Visual Tracking](https://www.diva-portal.org/smash/record.jsf?pid=diva2%3A785778&dswid=-4498) DSST 算法，只使用 HOG 特征，DCF 专门用于平移位置检测，并且，又专门训练了类似 MOSSE 的相关滤波器检测尺度变化，创造性地使用了平移滤波+尺度滤波。之后，提出 [2015-TPAMI-fDSST-Discriminative Scale Space Tracking](https://ieeexplore.ieee.org/abstract/document/7569092) fDSST 算法对DSST的加速优化，速度提升50%+，精度提升6%+.

进一步，Martin Danelljan 提出 [2016-ECCV-C-COT-Beyond Correlation Filters: Learning Continuous Convolution Operators for Visual Tracking](https://www.cvl.isy.liu.se/research/objrec/visualtracking/conttrack/C-COT_ECCV16.pdf) C-COT (Continuous Convolution Operator Tracker) 基于DCF，提出**训练连续卷积滤波器**，在**连续**空间域中，用**隐式插值模型**训练。

更进一步，Martin Danelljan 提出 [2017-CVPR-ECO-Efficient Convolution Operators for Tracking](https://openaccess.thecvf.com/content_cvpr_2017/html/Danelljan_ECO_Efficient_Convolution_CVPR_2017_paper.html) ECO 在 C-COT 的基础上进一步提高速度。

## 深度学习

DLT

MDNet

SiameseFC

SiameseRPN

DaSiameseRPN

SiameseRPN++

# 代表性学者

## University of Oxford 英国牛津大学

[**Joao F. Henriques**](https://www.robots.ox.ac.uk/~joao/) 和 [**Luca Bertinetto**](https://www.robots.ox.ac.uk/~luca/)

代表作：CSK, KCF, DCF, Staple, SiamFC, CFNet, Learnet

## Linköping University 瑞典林雪平大学 & the Computer Vision Lab at ETH Zurich, Switzerland.瑞士苏黎世联邦理工学院计算机视觉实验室

[**Martin Danelljan**](https://martin-danelljan.github.io/)

代表作：CN, DSST, SRDCF, DeepSRDCF, SRDCFdecon, C-COT, ECO, ATOM, DiMP, PrDiMP, KeepTrack

## 中国科学院

[**朱政**](http://www.zhengzhu.net/) （目前清华在读博后）

代表作：UCT, DaSiameseRPN

## 大连理工大学 智能图像分析与理解实验室（IIAU-LAB, Intelligent Image Analysis and Understanding Lab）

[卢胡川](http://ice.dlut.edu.cn/lu/index.html)

代表作：Online Visual Tracking, LTMU, STARK

# 参考链接

1. [VOT, OTB——目标追踪的发展概况](https://blog.csdn.net/dengheCSDN/article/details/78896933)

