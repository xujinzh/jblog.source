---
title: Centos 修改时区
author: Jinzhong Xu
mathjax: true
date: 2020-04-02 11:58:10
tags:
- centos
- linux
categories: technology
---

Centos修改系统显示时间可以通过修改时区来做。常用的时区一般包含CST, UTC, GMT.

<!--more-->

# 时区介绍

CST：China Standard Time，即中国标准时间，又称北京时间。是我中国大陆的标准时间，比世界协调时快八小时（即UTC+8），与香港、澳门、台湾、马来西亚、新加坡等地的标准时间相同。

UTC：**C**oordinated **U**niversal **T**ime，即协调世界时。是最主要的世界时间标准，其以原子时秒长为基础，在时刻上尽量接近于格林威治标准时间，UTC + 8小时 = CST。协调世界时是最接近格林威治标准时间（GMT）的几个替代时间系统之一。对于大多数用途来说，UTC时间被认为能与GMT时间互换，但GMT时间已不再被科学界所确定。

GMT：**G**reenwich **M**ean **T**ime，即格林威治平均时间。是指位于英国伦敦郊区的皇家格林威治天文台当地的平太阳时，因为本初子午线被定义为通过那里的经线。

# 查看时区

下面介绍如何修改系统时间，假设Centos系统采用UTC时区，可以通过如下命令查看

```bash
date
```

# 修改时区

我们想要修改为北京时间，方法如下

## 使用timedatectl

```bash
timedatectl set-timezone Asia/Shanghai
```

## 使用localtime

```bash
cp  /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
```

## 使用tzselect

```bash
tzselect
```

# 修改时间

如果，系统显示时间的分秒出现错误，可以使用如下命令修改

```bash
date -s '2020-04-02 12:20:30'
```

也可以根据网络自动同步，如果可以联网，使用ntp同步标准时间，ntp：网络时间协议（network time protol）

```bash
yum install ntp
ntpdate pool.ntp.org
```

