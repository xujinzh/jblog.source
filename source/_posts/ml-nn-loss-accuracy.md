---
title: 机器学习神经网络模型loss和accuracy的理解
author: Jinzhong Xu
mathjax: true
date: 2020-03-26 11:01:44
tags:
- machine learning
- neural network
categories: research
---

训练机器学习模型时，特别是使用 TensorFlow 进行训练神经网络模型时，在每一步（epoch，表示一个周期，即训练完所有训练集样本。假设训练集包含100000个样本，而batch_size设置为100，即一次输入100个训练样本，那么需要1000次迭代后就完成一次epoch，将所有训练样本训练一次）总是会出现loss是多少，accuracy是多少。

<!--more-->

loss越低，模型越好（当模型没有过拟合时）。它不是一个百分比，而是一次epoch中训练集样本或验证集样本所有error的和。比如，在神经网络中，loss值一般是 negative log likelihood(分类问题)和 residual sum of squares(回归问题)。因此，模型的训练是通过减少loss为目的的，即最小化loss function，如神经网络中采用反向传播对模型参数求导。loss隐含着每一次优化迭代后模型表现的好坏。

accuracy是当模型的参数学习得到并固定后来计算得到的。通过比较真实标签和预测标签来确定的0-1损失。它是一个百分比值。

优化训练的目标是loss值，而不是accuracy，因为loss function可以求导优化，而accuracy不能够求导。

