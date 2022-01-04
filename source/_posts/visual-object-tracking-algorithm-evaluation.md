---
title: 视觉目标跟踪算法评估
author: Jinzhong Xu
mathjax: true
date: 2020-08-28 12:57:57
tags:
- vot
- cv
- otb
categories:
- research
- object tracking
---

机器学习算法的评估指标常见的有精确率（Precision，精确率被定义为所有被预测成正样本的样本中真实的正样本比率）、召回率（Recall，召回率被定义为所有真实的正样本中被预测成正样本的样本比率）、F1值、ROC 和 AUC 等，但目标跟踪单根据这些指标是不能够满足跟踪器算法的评估的，它常使用帧率（FPS，每秒处理帧数）、IOU（Intersection Over Union），在 VOT 中提出的 Accuracy、Robustness、EAO 以及在 OTB 中提出的 Success plots of OPE、Precision plots of OPE、 SRE、TRE 来评价一个跟踪算法在一段视频上的跟踪性能等。

<!--more-->

# IOU 的定义

目标跟踪各算法常用矩形边界框（Bounding Box）来标注跟踪的目标，使用 IOU 进行计算矩形边界框的正确性比较容易，这也是使用矩形边界框来标注目标的原因之一。IOU 的定义如下：
$$
IOU = \frac{bb_1 \cap bb_2}{bb_1 \cup bb_2}.
$$
交、并表示区域的像素数目，IOU 值越大表示两个矩形边界框重合度越高，即跟踪效果越好。

# Accuracy 的定义

很多目标跟踪的竞赛都采用 Accuracy（准确率，简写 A）作为评价指标，其定义如下：
$$
Accuracy = \frac{1}{N}\sum^N_i IOU_i.
$$
Accuracy 值表示了一段视频中每一帧预测的目标矩形边界框与当前帧的 Ground Truth 矩形边界框之间的 IOU 的算术平均值，其中 $N$ 表示视频总帧数，或多段视频的总帧数或测试多次。Accuracy 越大表示跟踪效果越好。

# Robustness 的定义

目标跟踪很多时候会因为一些原因导致预测矩形边界框与 Ground Truth 矩形边界框没有交叠或交叠率达不到设定的阈值，此时，就是跟丢目标，好的跟踪算法需要防止这种情况发生，衡量标准就是 Robustness（鲁棒性，简写R），定义如下：
$$
Robustness = \frac{Number Of Failed Frames}{N}.
$$
Robustness 表示跟踪失败（当前预测目标的矩形边界框与 Ground Truth 矩形边界框的 IOU 值为0或小于某个设定的阈值）的帧数与总帧数的比值，该值越小表示跟踪算法越鲁棒或越稳健。

# EAO 的定义

很多公开赛（如 VOT，Visual-Object-Tracking Challenge，开始于 2013 年，由伯明翰大学、卢布尔雅那大学、布拉格捷克技术大学、奥地利科技学院联合创办，旨在评测在复杂场景下单目标跟踪的算法性能。）都会使用 EAO（Expected Average Overlap）来作为最终衡量跟踪算法的性能，它权衡了 Accuracy 和 Robustness，具体的计算公式如下：
$$
EAO = E(Accuracy) = \frac{1}{N}(\sum^N_i Accuracy_i) = \frac{1}{N}\sum^N_i(\frac{1}{N_i}\sum^{N_i}_j IOU_j)
$$

举个例子：假设一个视频就两帧，那么 $N=2$，因为第一帧的矩形边界框是给定的，则 $IOU_1 = 1$，假设第二帧的 $IOU_2 = 0.6$，那么该视频的 $Accuracy_1 = \frac{1}{1} \sum IOU_1 = 1$, $Accuracy_2 = \frac{1}{2}\sum_1^2(IOU_1 + IOU_2) = 0.8$， 
$$
EAO = \frac{1}{2}\sum_1^2(Accuracy_1 + Accuracy_2) = 0.9.
$$
在 VOT 挑战赛中，跟丢的帧将被重新初始化，再次进行跟踪，计算 EAO 时也会将视频按照跟丢的帧切割为多个视频段，最终 EAO 为各视频段的平均 EAO. 当跟丢的帧数发生在视频帧前时计算的 EAO 会比发生在后面帧跟丢情况的更低。

# Success plots 的定义

跟踪算法预测的矩形边界框与 Ground Truth 矩形边界框的重叠率，即上面定义的 $IOU$，当某一帧的重叠率大于设定的阈值，则认为该帧的跟踪是成功的。总成功帧占所有帧的百分比即为成功率（Success rate）。Success plots 是一张图，纵坐标为成功率，横坐标为重叠率阈值（Overlap threshold），从 $0$ 到 $1$。成功率随着阈值的变化而变化，一般是递减的，因此，得到一张图。一般在图例上会给出计算得到的成功率曲线下面积值。

# Precision plots 的定义

跟踪算法预测的矩形边界框的中心点与 Ground Truth 矩形边界框的中心点之间的距离为 D，当某一帧的距离 D 小于设定的位置错误阈值，则认为该帧的跟踪精度高。所有距离小于阈值的帧的总数占比视频总帧数的比值即为精确率（Precision rate）。Precision plots 是一张图，纵坐标为精确率，横坐标为位置错误阈值（Location error threshold）。精确率随着位置错误阈值的变化而变化，一般是递增的，因此，得到一张图。一般在图例上会给出计算得到的精确率曲线下面积值。

# OPE

用 Ground Truth 中目标的位置初始化第一帧，然后运行跟踪算法得到精确率图和成功率图，这种方法被称为 One-Pass Evaluation (OPE)。

# SRE 和 TRE

SRE （Spatial Robustness Evaluation）通过从空间（Spatially，不同的 Bounding Box）上打乱再进行评估；TRE（Temporal Robustness Evaluation）通过从时间（Temporally，从不同帧起始）上打乱再进行评估。

在一个视频中，每个跟踪算法从不同的帧作为起始进行追踪（比如分别从第一帧开始进行跟踪，从第十帧开始进行跟踪等），初始化采用的 Bounding Box 即为对应帧标注的 Ground Truth. 最后对这些结果取平均值，得到 TRE score.

由于有些算法对初始化时给定的 Bounding Box 比较敏感，而目前测评用的 Ground Truth 都是人工标注的，因此可能会对某些跟踪算法产生影响。因此为了评估这些跟踪算法是否对初始化敏感，作者通过将 Ground Truth 轻微的平移和尺度的扩大与缩小来产生Bounding Box.平移的大小为目标物体大小的 10%，尺度变化范围为 Ground Truth 的 80% 到 120%，每 10% 依次增加。最后取这些结果的平均值作为SRE score.

# 评估数据集

评估数据集对于目标跟踪算法同样很重要。常用的数据集包括 VOT（从 2013 年开始，每年一批数据集）, OTB（分为 OTB50、OTB100）, UAV, GOT-10k. 针对深度学习预训练数据集还包括 ImageNet, COCO 等。

评估数据集需要满足一些条件才能有效评估目标跟踪算法的优劣，如视频数量充足、目标类别丰富、以及标注信息准确等。

# 参考文献

1. [视觉目标跟踪漫谈：从原理到应用](https://developer.aliyun.com/article/766769)

2. [VOT中的EAO是如何计算的](https://blog.csdn.net/sinat_27318881/article/details/84350288)

3. [视觉跟踪比赛VOT评价指标的计算](https://zhuanlan.zhihu.com/p/83291192)

4. [【技术向】VOT中的EAO是如何计算的](https://blog.csdn.net/sinat_27318881/article/details/84350288)

5. [OTB评估指标](https://zhuanlan.zhihu.com/p/60262450)

   



