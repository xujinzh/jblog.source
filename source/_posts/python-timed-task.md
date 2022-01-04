---
title: Python 定时任务
mathjax: true
author: Jinzhong Xu
date: 2021-10-27 15:29:21
tags:
- python
- time
- timer
- schedule
- apscheduler
categories:
- technology
- python
---

利用计算机能够帮助我们执行定时定点或者间隔时间段的任务，特别是对于容易遗忘、重复操作、准点执行等任务非常方便。除了在 Linux 上使用 corntab 等完成之外，在 Python 中也可以编程实现定时定点等任务的执行。本篇介绍 time, threading.Timer, schedule, apscheduler 等四个模块。

<!--more-->

# Time

定时 3 秒后，依次执行相同任务 5 次

```python
from datetime import datetime
import time


def showTime(count):
    time.sleep(count)
    print(datetime.now().strftime("%Y/%m/%d %H:%M:%S"))

# 使用 while 可永久执行
for i in range(5):
    showTime(3)
```

# Threading.Timer

定时 3 秒后，同时执行相同任务 5 次

```python
from datetime import datetime
from threading import Timer


def showTime():
    print(datetime.now().strftime("%Y/%m/%d %H:%M:%S"))

# 使用 while 可永久执行
for i in range(5):
    count = 3
    t = Timer(count, showTime)
    t.start()
```

# schedule

每个 3 秒，执行任务

```python
import time
import schedule


def showTime():
    print(datetime.now().strftime("%Y/%m/%d %H:%M:%S"))


schedule.every(3).seconds.do(showTime)

while True:
    schedule.run_pending()
    time.sleep(1)
```

# apscheduler 

定时顶点/间隔执行任务

```python
from datetime import datetime
from apscheduler.schedulers.blocking import BlockingScheduler


# 定制执行任务
def showTime():
    print(datetime.now().strftime("%Y/%m/%d %H:%M:%S"))


# 创建调度器对象，并设置时区
scheduler = BlockingScheduler(timezone="Asia/Shanghai")

# 分配任务
## 使用 cron，类似 Linux 中 crontab
# scheduler.add_job(func=showTime, trigger="cron", day_of_week='0-6', hour=14, minute=15, second=1)
scheduler.add_job(func=showTime, trigger="cron", second='*/3') # 每隔 3 秒执行一次
## 固定间隔
# scheduler.add_job(func=showTime, trigger="interval", seconds=3)
## 定点执行
# scheduler.add_job(func=showTime, trigger='date', run_date='2021-10-27 15:48:05', args=[])

# 启动调度器
scheduler.start()
```

# 参考链接

1. [你知道 Python 中的定时任务吗？](https://www.cnblogs.com/traditional/p/10991231.html)
2. [python - 如何通过APScheduler 3.0的UTC时区？](https://www.coder.work/article/2034431)
3. [APScheduler定时框架](https://www.cnblogs.com/-wenli/p/12790257.html)
4. [Python任务调度模块APScheduler](https://segmentfault.com/a/1190000011084828)

<iframe src="//player.bilibili.com/player.html?aid=976240224&bvid=BV1v44y1v7jA&cid=428973248&page=13" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
