---
title: 把 Python 的 print 函数输出到文件
mathjax: true
author: Jinzhong Xu
date: 2021-11-28 20:16:26
tags:
- python
categories:
- technology
- python
---

利用 Python 的 print 函数可以方便的打印输出，但是，这些输出不能够很好的给别人传看，特别是打印行数较多，又不便更改原模块的代码时。这里，给出一个方法，不动原模块，将打印输出到文件中。

<!--more-->

# 所有打印都输入文件

假设，我们打印输出的模块是 main.py 里的 inspect 函数。这里创建 test.py 模块

```python
import sys
from main import inspect

sys.stdout = open('output.txt', 'w')
inspect(ids_dir)
sys.stdout.close()
```

或者

```python
import sys
from main import inspect

original_stdout = sys.stdout
with open('output.txt', 'w') as f:
    sys.stdout = f # Change the standard output to the file we created.
    inspect(ids_dir)
sys.stdout = original_stdout # Reset the standard output to its original value
```

# 有选择的打印输出到文件

如果想要有选择性的把打印内容输出到文件，可以如下：

```python
print("Hello Python!", file=open('output.txt','a'))
print("We are printing to file.", file=open('output.txt','a'))
```



# 参考链接

1. [Writing to a File with Python's print() Function](https://stackabuse.com/writing-to-a-file-with-pythons-print-function/)
2. [How to redirect 'print' output to a file?](https://stackoverflow.com/questions/7152762/how-to-redirect-print-output-to-a-file)
3. [How to redirect print output to a text file in Python](https://www.kite.com/python/answers/how-to-redirect-print-output-to-a-text-file-in-python)
