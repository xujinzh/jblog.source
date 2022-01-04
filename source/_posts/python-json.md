---
title: Python JSON
mathjax: true
author: Jinzhong Xu
date: 2021-04-14 14:55:31
tags:
- python
- json
- dict
categories:
- technology
- python
---

在该篇将学习如何在 Python 中解析、读取、写入 JSON，同时，将介绍如何把 JSON 转化为字典并格式化打印。

JSON（JavaScript Object Notation）是表示结构化数据的流行数据结构。通常服务器和网络应用之间以 JSON 格式发送和接受数据。

在 Python 中，JSON 以字符串格式存在，如：

```python
para = '{"model": "VGG", "optim_kwargs": {"lr": 1e-2, "momentum": 0.9}}'
```

通常，JSON 对象保存在文件中。

<!--more-->

# 把 JSON 字符串转化为字典

```python
>> import json

>> para = '{"model": "VGG", "optim_kwargs": {"lr": 1e-2, "momentum": 0.9}}'
>> para_dict = json.loads(para)
>> print(para_dict)
>> print(para_dict["model"])
{'model': 'VGG', 'optim_kwargs': {'lr': 0.01, 'momentum': 0.9}}
VGG
```

# 把字典保存为JSON文件

```python
>> import json

>> para_dict = {'model': 'VGG', 'optim_kwargs': {'lr': 0.01, 'momentum': 0.9}}
>> with open('./data/paras.json', 'w') as jf:
...    json.dump(para_dict, jf, indent=4, sort_keys=True, ensure_ascii=False)
```

# 从JSON文件中读取数据成字典

```python
>> import json

>> with open("./data/paras.json") as jf:
>>     para_dict = json.load(jf)

>> print(para_dict["model"])
VGG
```

# 把字典转化为JSON字符串

```python
>> import json

>> para_dict = {"model": "VGG", "optim_kwargs": {"lr": 0.01, "momentum": 0.9}}

>> para = json.dumps(para_dict, ensure_ascii=False)
>> print(para)
# 注意JSON字符串不同于字典，是不能够直接索引的
```

# 记忆方法

1. 字典是 JSON 字符串和 JSON 文件的桥梁；
2. JSON 字符串和字典之间的转化用带 's' 的方法；
3. JSON 文件和字典之间的转化用不带 's' 的方法；
4. JSON 到字典用 'load';
5. 字典到 JSON 用 'dump'；

表示为如下：

$JSON 字符串 \overset{dumps}{\longleftarrow} 字典 \overset{dump}{\longrightarrow} JSON 文件$

$JSON 字符串 \overset{loads}{\longrightarrow} 字典 \overset{load}{\longleftarrow} JSON 文件$

合并表示如下：

$JSON 字符串 \underset{loads}{\overset{dumps}{\leftrightarrows}} 字典 \underset{load}{\overset{dump}{\rightleftarrows}} JSON 文件$

保存含有中文的字符，请注意参数 **ensure_ascii=False**

# 参考链接

1. [Python JSON](https://www.programiz.com/python-programming/json)

2. [Python下json中文乱码解决办法](https://blog.csdn.net/ys_073/article/details/9403039)

   

