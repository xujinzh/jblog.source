---
title: 单目标跟踪数据集
mathjax: true
author: Jinzhong Xu
date: 2021-12-14 16:22:56
tags:
- datasets
- object tracking
categories:
- research
- object tracking
---

目标跟踪数据集有很多，这里汇集单目标跟踪数据集，给出链接以及数据集说明。

<!--more-->

# LaSOT

**La**rge-scale **S**ingle **O**bject **T**racking (**LaSOT**) 是天普大学，华南理工大学，鹏城实验室，美图-亮风台联合实验室等提出，第一作者是 [Heng Fan](hefan@cs.stonybrook.edu) ，该数据集数据量大，用于评估长时跟踪性能，特点如下：

- **Large-scale:** 1,550 sequences with more 3.87 millions frames
- **High-quality:** Manual annotation with careful inspection in each frame
- **Category balance:** 85 categories with each containing twenty (70 classes) or ten (15 classes) sequences
- **Long-term tracking:** An average video length of around 2,500 frames (i.e., 83 seconds)
- **Comprehensive labeling:** Providing both visual and lingual annotation for each sequence
- **Flexible Evaluation Protocol:** Evaluation under three different protocols: no constraint, full-overlap and one-shot

相关文章：

[LaSOT: A High-quality Large-scale Single Object Tracking Benchmark](https://arxiv.org/abs/2009.03465)
H. Fan*, H. Bai*, L. Lin, F. Yang, P. Chu, G. Deng, S. Yu, Harshit, M. Huang, J Liu, Y. Xu, C. Liao, L Yuan, and H. Ling
*International Journal of Computer Vision (**IJCV**)*, 2020. (accepted)

[LaSOT: A High-quality Benchmark for Large-scale Single Object Tracking](https://arxiv.org/abs/1809.07845)
H. Fan*, L. Lin*, F. Yang*, P. Chu*, G. Deng, S. Yu, H. Bai, Y. Xu, C. Liao, and H. Ling
*IEEE Conference on Computer Vision and Pattern Recognition (**CVPR**)*, 2019.

相关博客：[亮风台](https://www.leiphone.com/category/academic/NhOeLVXzIdLVSxyU.html)

官方数据集下载地址：http://vision.cs.stonybrook.edu/~lasot/download.html

类别：70

视频：1400

大小：大约227G

注释：边界框、完全遮挡、视野外标志以及语言描述

边界框： `[x, y, width, height] `

评估方法：one-pass evaluation (OPE)

评估度量：**Precision**, **Normalized Precision** and **Success**

评估工具集：https://github.com/HengLan/LaSOT_Evaluation_Toolkit

# GOT-10k

**G**eneric **O**bject **T**racking (GOT-10k) 是有中科院自动化所提出，第一作者是Lianghua Huang，该数据集是一个用于野外通用对象跟踪的大型、高多样性、一次性（one-shot）数据集。

相关文章：

GOT-10k: A Large High-Diversity Benchmark for Generic Object Tracking in the Wild.
L. Huang*, X. Zhao*, and K. Huang. *( \*Equal contribution)*
*IEEE Transactions on Pattern Analysis and Machine Intelligence (TPAMI).*
[[PDF](https://arxiv.org/abs/1810.11981)] [[BibTex](http://got-10k.aitestunion.com/bibtex)]

官方数据集下载地址：http://got-10k.aitestunion.com/downloads

类别：563目标类，87运动类

图像：10k

大小：66G

注释：边界框、对象可见率、对象存在与否、对象是否在每帧中被图像切割以及序列的元信息（对象、运动类、URL）；注释采用 txt 文档

边界框： `[xmin, ymin, width, height] `

评估方法：one-shot protocol

评估度量：mAO (mean average overlap)， mSR (mean success rate)

评估工具集：https://github.com/got-10k/toolkit

# COCO

**C**ommon **O**bjects in **Co**ntext (COCO) 是微软提出的，该数据集是一个大规模的对象检测（object detection）、分割（segmentation）和图像标注（captioning，看图说话）数据集。特点如下：

- 图片大多数来源于生活中，背景更复杂
- 每张图片上的实例目标个数多，平均每张图片7.7个
- 小目标更多
- 评估标准更严格

相关博客：[目标检测数据集MSCOCO简介](https://arleyzhang.github.io/articles/e5b86f16/)

相关文章：

Lin, T. Y., Maire, M., Belongie, S., Hays, J., Perona, P., Ramanan, D., ... & Zitnick, C. L. (2014, September). Microsoft coco: Common objects in context. In *European conference on computer vision* (pp. 740-755). Springer, Cham.

[[PDF](https://link.springer.com/content/pdf/10.1007/978-3-319-10602-1_48.pdf)]

官方数据集下载地址：https://cocodataset.org/#download

类别：80目标类，91 stuff categories, 5 captions per image, 250k people with keypoints

图像：330K（>200K 标注）；1.5 million 目标实例；平均每张图片包含的3.5个类别和7.7个目标；小目标占比较ImageNet多；包含大约 41% 的小目标 ($\textbf{area} < 32 \times 32$), 34% 的中等目标 ($ 32 \times 32 < \textbf{area} < 96 \times 96$) 和 24% 的大目标 ($\textbf{area} > 96 \times 96$).

大小：46G（2017）

注释：包含id, image_id, category_id, segmentation, area, iscrowd, bbox；注释采用JSON文件

边界框： `[x, y, width, height] `

评估方法：one-pass evaluation (OPE)

评估度量：mAP (mean average precision)， mAR (mean average recall)

评估工具集：https://github.com/cocodataset/cocoapi

# TrackingNet

TrackingNet 是阿卜杜拉国王科技大学提出的，该数据集是野外目标跟踪的大规模数据集和基准。

相关博客：[目标跟踪数据集整理（一）---TrackingNet](https://blog.csdn.net/xwmwanjy666/article/details/98525030)

相关文章：

Muller, M., Bibi, A., Giancola, S., Alsubaihi, S., & Ghanem, B. (2018). Trackingnet: A large-scale dataset and benchmark for object tracking in the wild. In *Proceedings of the European Conference on Computer Vision (ECCV)* (pp. 300-317).

[[PDF](https://openaccess.thecvf.com/content_ECCV_2018/papers/Matthias_Muller_TrackingNet_A_Large-Scale_ECCV_2018_paper.pdf)]

官方数据集下载地址：https://tracking-net.org/

类别：23目标类，人分为7个详细类别

视频：>30K；平均时长为16.6s，1443,1266 帧

大小：1.1T

注释：scale variation; aspect ratio change; fast motiong; low resolution; out-of-view etc.

边界框： `[x, y, width, height] `

评估方法：one-pass evaluation (OPE)

评估度量：**success S**， **precision P**， **Pnorm**

评估工具集：https://github.com/SilvioGiancola/TrackingNet-devkit

