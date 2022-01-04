---
title: OpenCV (Python) 中 BGR 和 RGB 的转换
mathjax: true
author: Jinzhong Xu
date: 2021-10-03 16:16:16
tags:
- opencv
- python
categories:
- research
- object tracking
---

在目标跟踪中经常会使用到 OpenCV 和 Pillow 库等来处理图像。但是，它们处理图像的方式有所不同，比如 OpenCV 把图像看作一个 `ndarray` 数组对象，而 Pillow 把图像看作一个自定义的 `Image` 对象；OpenCV 中图像是以 `BGR` (blue, green , red) 顺序存储的，而 Pillow 中的图像是以 `RGB` (red, green , blue) 存储的。因此，如果想要同时使用两种库的函数，需要进行相应的 `BGR` 和 `RGB` 的转换。

<!--more-->

OpenCV 中图像的 `BGR` 和 `RGB` 的转换可以有两种方法：

1. 通过 OpenCV 内置的 `cvtColor()` 函数;
2. 直接改变 `ndarray` 的存储顺序。

# 图像读取与写入

```python
# 导入包
import cv2
import numpy as np
from PIL import Image

# OpenCV 中以 BGR 的顺序读取图像 ndarray 对象，以同样方式写入。
# 打开时才可正常显示
im_bgr_cv = cv2.imread('data/lena.jpg')
cv2.imwrite('data/dst/lena_bgr_cv.jpg', im_bgr_cv)

# Pillow 以 RGB 的顺序读取图像 Image 对象，以同样方式写入
# 打开时才可正常显示
im_rgb_pil = Image.fromarray(im_bgr_cv[:, :, ::-1])
im_rgb_pil = Image.fromarray(im_bgr_cv[:, :, [2, 1, 0]])
im_rgb_pil = Image.fromarray(cv2.cvtColor(im_bgr_cv, cv2.COLOR_BGR2RGB))
im_rgb_pil.save('data/dst/lena_rgb_pil.jpg')
```

# cvtColor() 转化 BGR 和 RGB

在 OpenCV 中, 不同的颜色空间(如 `RGB, BGR, HSV`) 可以利用函数 `cvtColor()` 函数进行转换

```python
dst = cv2.cvtColor(src, code)
# src 表示转换前的图像
# dst 表示转换后的图像
# code 表示转换方式, 如 cv2.COLOR_BGR2RGB、cv2.COLOR_RGB2GBR
```

## BGR 转换为 RGB

```python
im_rgb_cv = cv2.cvtColor(im_bgr_cv, cv2.COLOR_BGR2RGB)
im_rgb_pil = Image.fromarray(im_rgb_cv)
# 在 Pillow 中处理
im_rgb_pil.save('data/dst/lena_rgb_pillow.jpg')
```

## RGB 转换为 BGR 

```python
im_rgb_pillow = Image.open('data/src/lena.jpg')
im_bgr = cv2.cvtColor(np.array(im_rgb_pillow), cv2.COLOR_RGB2BGR)
# 在 OpenCV 中处理
cv2.imwrite('data/dst/lena_bgr_cv_2.jpg', im_bgr)
```

# 直接在 ndarray 中转换

```python
im_bgr = cv2.imread('data/src/lena.jpg')

im_rgb = im_bgr[:, :, [2, 1, 0]]
Image.fromarray(im_rgb).save('data/dst/lena_swap.jpg')

im_rgb = im_bgr[:, :, ::-1]
Image.fromarray(im_rgb).save('data/dst/lena_swap_2.jpg')
```

# 参考链接

1.[Convert BGR and RGB with Python, OpenCV (cvtColor)](https://note.nkmk.me/en/python-opencv-bgr-rgb-cvtcolor/)

