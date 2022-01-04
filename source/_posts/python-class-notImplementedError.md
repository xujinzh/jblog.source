---
title: Python中NotImplementedError的使用
mathjax: true
author: Jinzhong Xu
date: 2021-12-16 14:30:32
tags:
- python
categories:
- technology
- python
---

Python 是一种面向对象的编程语言，子类可以继承父类的方法。父类可以通过某种手段限制子类中必须重写父类的指定方法，否则报错。这种手段就是 NotImplementedError.

<!--more-->

# 如果子类不重写父类的方法

```python
class ParentClass:
    def nie_func(self):  # 要求子类中必须实现该方法，否则在子类实例中不能调用该方法
        raise NotImplementedError("parent class nie methods not implemented")


class ChildClass(ParentClass):
    pass


cc = ChildClass()
cc.nie_func()
```

结果如下：

```python
---------------------------------------------------------------------------
NotImplementedError                       Traceback (most recent call last)
/tmp/ipykernel_46111/872110974.py in <module>
      9 
     10 cc = ChildClass()
---> 11 cc.nie_func()

/tmp/ipykernel_46111/872110974.py in nie_func(self)
      1 class ParentClass:
      2     def nie_func(self):  # 要求子类中必须实现该方法，否则在子类实例中不能调用该方法
----> 3         raise NotImplementedError("parent class nie methods not implemented")
      4 
      5 

NotImplementedError: parent class nie methods not implemented
```

发现报错 NotImplementedError，即没有实现方法的错误。在下面示例中进行修复，在子类中实现父类的方法。

# 子类重写父类的方法

```python
class ParentClass:
    def nie_func(self):  # 要求子类中必须实现该方法，否则在子类实例中不能调用该方法
        raise NotImplementedError("parent class nie methods not implemented")


class ChildClass(ParentClass):
    def nie_func(self):  # 子类中重新父类方法
        print("child implement tdemo methods")


cc = ChildClass()
cc.nie_func()
```

结果如下：

```python
child implement tdemo methods
```

发现不再报 NotImplementedError.

# 参考链接

1.[Python编程中NotImplementedError的使用](https://blog.csdn.net/grey_csdn/article/details/77074707)
