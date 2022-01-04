---
title: 数学中集合在 Python 中的操作
mathjax: true
author: Jinzhong Xu
date: 2021-10-25 09:54:48
tags:
- set
- python
- maths
categories:
- technology
- python
---

在数学中，集合（Set）是一个基本概念，指具有某种特定性质的事物的总体，集合里的事物称作元素。元素 $x$ 和集合 $A$ 之间有属于（$x \in A$）和不属于（$x \notin A$）关系，集合有势（#$A$, $Card(A)$, $|A|$）的概念，集合有子集和包含关系（$B \subset A$），两集合 $A, B$ 间有并（$A\cup B$）、交（$A \cap B$）、差（$A - B$）、对称差（$A \triangle B$）运算。在 Python 中，对于该数学概念进行了定义，下面我们进行介绍。

<!--more-->

# 集合定义

```python
A = set('abc')
B = set(['c', 'd', 'e'])
C = {'c', 'm', 'n'}

A
{'a', 'b', 'c'}

B
{'c', 'd', 'e'}

C
{'c', 'm', 'n'}
```

# 集合关系

```python
A = set('abc')
B = set(['c', 'd', 'e'])
C = {'c', 'm', 'n'}

# A 是否包含于 B
A.issubset(B)
False

# A 是否包含 B
A.issuperset(B)
False

# A 和 B之间是否没有交。如果不交返回True，相交返回False
A.isdisjoint(B)
False
```

# 集合运算

```python
A = set('abc')
B = set(['c', 'd', 'e'])
C = {'c', 'm', 'n'}

# A 并 B
A | B
A.union(B)
set.union(A, B)
{'a', 'b', 'c', 'd', 'e'}

C.union(A, B)
A | B | C
{'a', 'b', 'c', 'd', 'e', 'm', 'n'}

# A 交 B
A & B
A.intersection(B)
{'c'}

# A 减 B
A - B
A.difference(B)
{'a', 'b'}

# A 和 B 的对称差
A ^ B
A.symmetric_difference(B)
{'a', 'b', 'd', 'e'}
```

# 集合增删

```python
A = set('abc')
B = set(['c', 'd', 'e'])
C = {'c', 'm', 'n'}

# A = A | B
A.update(B)
A
{'a', 'b', 'c', 'd', 'e'}

# A = A & B
A = set('abc')
A.intersection_update(B)
A
{'c'}

# A = A - B
A = set('abc')
A.difference_update(B)
A
{'a', 'b'}

# A = (A - B) | (B - A)
A = set('abc')
A.symmetric_difference_update(B)
A
{'a', 'b', 'd', 'e'}

# A = A | set('p')
A = set('abc')
A.add('p')
A
{'a', 'b', 'c', 'p'}

# 从 A 中移除 'p'，当 'p' 不在 A 中时将报错
A.remove('p')
A
{'a', 'b', 'c'}

# 从 A 中移除 'p'，当 'p' 不在 A 中时不会报错
A.discard('p')
```

# 集合元素

```python
A = {'a', 'b', 'c'}

# 集合的势，即集合中元素的个数
len(A)
3

# 判断元素是否在集合中
'p' in A
False

# 判断元素是否不在集合中
'p' not in A
True
```

# 集合应用-去重

```python
a = [1, 1, 2, 3]
b = list(set(a))
b
[1, 2, 3]
```

# 集合拷贝与清除

```python
A = {'a', 'b', 'c'}

D = A.copy()
id(A), id(D)
(139951109280704, 139951109280256)

D
{'a', 'b', 'c'}

D.discard('c')
D
{'a', 'b'}

A
{'a', 'b', 'c'}

A.clear()
A
set()
```

# 冻结集合

冻结的集合元素无法改变

```python
Numbers = frozenset('0123456789')
Numbers
frozenset({'0', '1', '2', '3', '4', '5', '6', '7', '8', '9'})
```

# 参考链接

1. [python 集合比较（交集、并集，差集）](https://blog.csdn.net/isoleo/article/details/13000975)

