---
title: 利用 OpenCV(Python) 读取网上的图片和视频
mathjax: true
author: Jinzhong Xu
date: 2021-10-03 17:18:03
tags:
- opencv
- python
- image
categories:
- research
- object tracking
---

在进行目标跟踪研究中,有时候需要直接读取网上的图片和视频,这时候利用 OpenCV 再加上 `urllib`、`pafy`、`numpy` 等就可以直接获取互联网上的各种图片和视频资源.

<!--more-->

# 从网上读取图片

```python
import cv2
import numpy as np
import urllib

url_img = 'https://img0.baidu.com/it/u=1073044967,3931799334&fm=26&fmt=auto'
res = urllib.request.urlopen(url_img)
i = np.asarray(bytearray(res.read()), dtype="uint8")
img = cv2.imdecode(i, cv2.IMREAD_COLOR)

# 彩色图像
plt.imshow(cv2.cvtColor(img, cv2.COLOR_BGR2RGB))
plt.imshow(img[:, :, ::-1])
# plt.imshow(img[:, :, [2, 1, 0]])

# 灰度图像
imgray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
plt.imshow(imgray, cmap='gray')

# 同时画多幅图
plt.imshow(np.hstack((imgray, imgray)), cmap='gray')

# 不显示坐标轴
plt.xticks([]), plt.yticks([])

plt.show()
```

彩色图片显示如下:

![](https://img0.baidu.com/it/u=1073044967,3931799334&fm=26&fmt=auto)

```python
# h, w, c 分别是高、宽、通道
img.shape
# (360, 450, 3)
```

# 从网上读取视频

```python
import pafy
import cv2

# The YouTube url or 11 character video id of the video
url_video = 'https://youtu.be/2kS0T7fwxJo'
# Preferred type, set to mp4, webm, flv, 3gp or any
# 把分享的视频地址,转化为 .mp4 地址,如何 cv2.VideoCapture() 直接读取
v = pafy.new(url_video).getbestvideo(preftype='mp4')
# 如果地址是"*.mp4"的,可以直接读取
vc = cv2.VideoCapture(v.url)

if vc.isOpened():
    open = True
else:
    open = False
flag = 0
while open:
    ret, frame = vc.read()
    if frame is None:
        break
    if ret:
        gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
        flag += 1
vc.release()
"total frame is ", flag
```

# 参考链接

1.[Pafy Documentation](https://pythonhosted.org/pafy/)

