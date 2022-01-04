---
title: 分组卷积、深度卷积和全局深度卷积
mathjax: true
author: Jinzhong Xu
date: 2021-07-02 18:20:22
tags:
- convolution
categories:
- research
- object tracking
---

卷积能够提取图像特征，不同的卷积操作能够获得不同的效果，同时有些卷积能够降低待学习的参数量，达到正则化的目的。

<!--more-->

# 分组卷积

分组卷积（Group Convolution）最早见于2012年 ImageNet 竞赛中的 AlexNet 网路中，被用来切分网络，使得模型能够在2个GPU上并行运行，AlexNet 的网络结构如下：

![](https://s2.ax1x.com/2019/01/08/FLPm1P.png)

## 分组卷积的定义

对于常规卷积，如果输入的特征图是 $C * H * W$，卷积核个数是 $N$，输出特征图的数量也是 $N$，其与卷积核的数量相同。假设卷积和的尺寸是 $C * K * K$，那么 $N$ 个卷积核的总参数量为 $N * C * K * K$，输入特征图与输出特征图的连接方式如下左图：

![](https://s2.ax1x.com/2019/01/08/FLPc1x.png)

分组卷积是对输入特征图先分组，然后每组分别卷积。假设输入特征图的尺寸仍为 $C * H * W$，输出特征图的数量为 $N$个，如果要分成 $G$ 个组，则每组的输入特征数量为  $\frac{C}{G}$，每组的输出特征图数量为 $\frac{N}{G}$，每个卷积核的尺寸为 $\frac{C}{G} * K * K$，卷积核的总数仍为 $N$个，每组的卷积核数量为 $\frac{N}{G}$，卷积核只与其同组的输入特征图进行卷积，卷积核的总参数量为 $N * \frac{C}{G} * K * K$，可见，总参数量减少为原来的 $\frac{1}{G}$，输入特征图与输出特征图的连接方式如上右图。针对 group1 （group2, group3 相同）的输入特征图是4个，因此其卷积核的通道数为4，输出特征图的通道数为2，表示有两个卷积核，卷积核不与其他组的输入特征图做卷积，只与同组的特征图卷积。

## 分组卷积的意义

1. <font size=3 color=red>减少参数量，分成 $G$组，则该层的参数量减少为原来的 $\frac{1}{G}$</font>
2. 分组卷积可以看成是 structured sparse，每个卷积核的尺寸从 $C * K * K$ 变为 $\frac{C}{G} * K *K$，可以将其余的 $(C - \frac{C}{G}) * K * K$的参数视为 0，有时甚至可以在减少参数量的同时获得更好的效果（相当于正则化）
3. 当分组数量等于输出特征图的数量时，即每一个通道的输入特征图一个分组，输出特征图的数量等于输入特征图的数量，即 $G = N = C$，$N$ 个卷积核每个尺寸为 $1 * K * K$是，分组卷积 Group Convolution 就变成了深度卷积 Depthwise Convolution，参数量进一步减少，参加 [MobileNet](https://arxiv.org/abs/1704.04861), [Xception](https://arxiv.org/abs/1610.02357). 如下图所示

![](https://s2.ax1x.com/2019/01/08/FLkxED.png)

4. 更进一步，如果分组数 $G = N = C$，即每个通道一个分组，同时卷积核的尺寸与输入特征图的尺寸相同，即 $K = H = W$，则输出特征图为 $C * 1 * 1$，得到长度为 $C$ 的向量，此时称之为  Global Depthwise Convolution (GDC)，参加 [MobileFaceNet](https://arxiv.org/abs/1804.07573)，可以看成是全局加权池化，与 Global Average Pooling (GAP) 的不同之处在于 GDC 给每个位置赋予可学习的权重（对于已经对齐的图像很有效，比如人脸，中心位置和边界位置的权重自然应该不同），而 GAP 每个位置的权重相同，全局取平均，如下图所示

   ![](https://s2.ax1x.com/2019/01/08/FLEneK.png)

# 参考连接

1. [Group Convolution分组卷积，以及Depthwise Convolution和Global Depthwise Convolution](https://www.cnblogs.com/shine-lee/p/10243114.html)
