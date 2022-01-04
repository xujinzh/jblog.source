---
title: 三维重建简单综述
date: 2019.12.21
tags:
- 3d
- study
- dl
categories: research
---

文章《<a rel="noreferrer noopener" aria-label="基于深度学习的视觉三维重建研究总结（在新窗口打开）" href="https://zhuanlan.zhihu.com/p/79628068" target="_blank">基于深度学习的视觉三维重建研究总结</a>》从三维重建的意义、定义、表示方式、分类开始，总结了深度学习在三维重建方向的最近几年（截至2019年）的进展，分别是

<!--more-->

<ol><li>从单张图像恢复深度图的文章《<a rel="noreferrer noopener" aria-label="Depth Map Prediction from a Single Image using a Multi-Scale Deep Network（在新窗口打开）" href="https://arxiv.org/abs/1406.2283" target="_blank">Depth Map Prediction from a Single Image using a Multi-Scale Deep Network</a>》，是深度学习进行三维重建的开山之作，由粗到细并提出尺度不变的损失函数。</li><li>单视图或多视图的体素三维重建文章《<a rel="noreferrer noopener" aria-label="3D-R2N2: A Unified Approach for Single and Multi-view 3D Object Reconstruction（在新窗口打开）" href="https://arxiv.org/abs/1604.00449" target="_blank">3D-R2N2: A Unified Approach for Single and Multi-view 3D Object Reconstruction</a>》结合编码解码网络和LSTM，既可以对单幅图像做三维图像体素重建，又可以对多幅图像做。</li><li>单张RGB图像的点云三维重建文章《<a rel="noreferrer noopener" aria-label="A Point Set Generation Network for 3D Object Reconstruction from a Single Image（在新窗口打开）" href="https://arxiv.org/abs/1612.00603" target="_blank">A Point Set Generation Network for 3D Object Reconstruction from a Single Image</a>》用1024个点云数据表示物体的三维重建，并提出了CD（Chamfer Distance）和 EMD（Earth Mover's Distance）。</li><li>单张RGB图像的Mesh三维重建文章《<a rel="noreferrer noopener" aria-label="Pixel2Mesh: Generating 3D Mesh Models from Single RGB Images（在新窗口打开）" href="https://arxiv.org/abs/1804.01654" target="_blank">Pixel2Mesh: Generating 3D Mesh Models from Single RGB Images</a>》从椭球体Mesh通过深度学习（CNN + GCN） 端到端 变形为对应物体的Mesh三维重建。</li><li>文章《<a rel="noreferrer noopener" aria-label="Mesh R-CNN（在新窗口打开）" href="https://arxiv.org/abs/1906.02739" target="_blank">Mesh R-CNN</a>》使用《<a rel="noreferrer noopener" aria-label="Mask R-CNN（在新窗口打开）" href="https://arxiv.org/abs/1703.06870" target="_blank">Mask R-CNN</a>》框架实现现实图片的物体检测，并为每个检测物体生成三角Mesh。文章使用了图卷积网络GCN和多种损失来训练。</li><li>文章《<a rel="noreferrer noopener" aria-label="Conditional Single-view Shape Generation for Multi-view Stereo Reconstruction（在新窗口打开）" href="https://arxiv.org/abs/1904.06699" target="_blank">Conditional Single-view Shape Generation for Multi-view Stereo Reconstruction</a>》使用条件生成模型对不可见部分进行建模。</li></ol>
