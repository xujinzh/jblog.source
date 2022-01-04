---
title: Python 中 deepcopy, copy, 赋值的区别
date: 2019.12.21
tags:
- python
- copy
categories: technology
---

Python 中数据对象分为三类，一类是不可变对象，如元组、字符串、数值；一类为单层可变对象，如列表 L = [1, 2, 3]，字典 D = {'x': 1, 'y': 2}；最后一类是多层可变对象，即可变对象中嵌套有可变对象，如列表 L = [1, [1, 2], 3]，字典 D = {'x': [1, 2], 'y' = 3}。

<!--more-->

Python 中拷贝也分为三类， 一类是赋值 '=';   一类是浅层拷贝 copy.copy(); 一类是深层拷贝 copy.deepcopy()，它们表现了对数据操作的不同程度。赋值'='是对数据对象“打上标签”, 而不对具体的数值操作，是一种指向操作。深层拷贝是真正意义上的拷贝一份，创造出两个完全意义的数据对象，在内存中开辟一个空间来存储数值。浅层拷贝对于对于不可变对象和单层可变对象开辟一个内存空间来存储数值，但是只存储“一层”，对于多层可变对象数据，不能够嵌套拷贝，因为嵌套的可变对象在多层可变对象中是以索引或标签的形式存储的。

举一些例子：

对于固定值的不可变对象，三者一样

```python
import copy
import numpy as np
x = 1
y_deep = copy.deepcopy(x)
y_copy = copy.copy(x)
y_assi = x
print(id(x))
print(id(y_deep))
print(id(y_copy))
print(id(y_assi))
```


```wiki
94442834912000
94442834912000
94442834912000
94442834912000
```

对于单层可变对象，三者不同

```python
x = [1, 2, 3]
y_deep = copy.deepcopy(x)
y_copy = copy.copy(x)
y_assi = x
print(id(x))
print(id(y_deep))
print(id(y_copy))
print(id(y_assi))
```

```wiki
139621304194544
139621670020192
139621671066192
139621304194544
```

对于多层可变对象，三者不同

```python
x = [1, [1, 2], 3]
y_deep = copy.deepcopy(x)
y_copy = copy.copy(x)
y_assi = x
print(id(x))
print(id(y_deep))
print(id(y_copy))
print(id(y_assi))
```

```wiki
139621669999392
139621671065792
139621669999792
139621669999392
```

多层可变对象，deepcopy() 和 copy() 的区别

```python
x = [1, [1, 2], 3]
y_deep = copy.deepcopy(x)
y_copy = copy.copy(x)
y_assi = x
x[1][0] = 9
print('x = ', x)
print('y_deep = ', y_deep)
print('y_copy = ', y_copy)
print('y_assi = ', y_assi)
```

```wiki
x =  [1, [9, 2], 3]
y_deep =  [1, [1, 2], 3]
y_copy =  [1, [9, 2], 3]
y_assi =  [1, [9, 2], 3]
```

<p>补充参考 <a rel="noreferrer noopener" aria-label="csdn（在新窗口打开）" href="https://blog.csdn.net/u011630575/article/details/78604226" target="_blank">csdn</a></p>