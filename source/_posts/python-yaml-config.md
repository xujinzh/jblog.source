---
title: 在 Python 中使用 yaml 做配置文件
mathjax: true
author: Jinzhong Xu
date: 2021-12-17 15:43:39
tags:
- python
categories:
- technology
- python
---

YAML (YAML Ain't Markup Language™) 是一种直观的能够被电脑识别的数据序列化格式，容易被人类阅读，并且容易和脚本语言交互。YAML 类似于 XML，但是语法比 XML 简单得多，对于转化成数组或可以 hash 的数据时是很简单有效的。

<!--more-->

yaml 的官方网站是：https://yaml.org

安装 yaml

```bash
pip install PyYAML
```

# 读取 yaml 文档

有文档 test.yaml，内容如下

```yaml
name: Tom Smith
age: 37
spouse:
    name: Jane Smith
    age: 25
children:
  - name: Jimmy Smith
    age: 15
  - name: Jenny Smith
    age: 12
```

在 python 中利用 yaml 读取

```python
import yaml

filepath = "data/test.yaml"
file = open(file=filepath)
file_str = yaml.load(file, yaml.Loader)
print(file_str)
```

结果如下

```yaml
{'name': 'Tom Smith',
 'age': 37,
 'spouse': {'name': 'Jane Smith', 'age': 25},
 'children': [{'name': 'Jimmy Smith', 'age': 15},
  {'name': 'Jenny Smith', 'age': 12}]}
```

# 把字典写入 yaml 文档

```python
f = open('data/test2.yaml', 'w')
yaml.dump(file_str, f)
```

