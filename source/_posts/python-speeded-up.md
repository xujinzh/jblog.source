---
title: Python 加速计算介绍
mathjax: true
author: Jinzhong Xu
date: 2020-12-02 10:48:45
tags:
- python
- multiprocess
categories:
- technology
- python
---

Python 编写程序简单高效，但运行效率相比 C 等较慢，不太适合处理计算密集型任务（对 IO 较适用）。如果想要适用 Python 进行密集计算，可以采用某些手段加速计算，一定程度上缓解这种矛盾。

<!--more-->

# Numba 模块

Numba is an open source JIT compiler that translates a subset of Python and NumPy code into fast machine code. 

Numba 是开源的 JIT 编译器，它通过 llvmlite Python 包，使用 LLVM 将 Python 的子集和 NumPy 翻译成快速的机器码。它为在 CPU 和 GPU 上并行化 Python 代码提供了大量选项，而经常只需要微小的代码变更。下面给出一个实例介绍 Numba 模块加速计算效果。

```python
import timeit

import numpy as np
from numba import jit, njit


def my_sum1(n=1000):
    return sum(range(n))


def my_sum2(n=1000):
    sum = 0
    for i in range(n):
        sum += i
    return sum


@njit
def my_sum3(n=1000):
    sum = 0
    for i in range(n):
        sum += i
    return sum


if __name__ == "__main__":
    n = 100
    print("内置函数优化耗时: {}s".format(timeit.timeit("my_sum1(%d)" % n, globals=globals(), number=100_0000)))
    print("普通循环函数耗时: {}s".format(timeit.timeit("my_sum2(%d)" % n, globals=globals(), number=100_0000)))
    print("jit加速后耗时: {}s".format(timeit.timeit("my_sum3(%d)" % n, globals=globals(), number=100_0000)))
```

输出结果如下：

```bash
内置函数优化耗时: 0.9235639999969862s
普通循环函数耗时: 3.137067300005583s
jit加速后耗时: 0.20853160000115167s
```

使用 Python 内置函数，减少循环能够一定程度上降低运行速度；使用加速模块 numba 在数值循环上能够大大降低运行速度。

# Multiprocessing 模块

[`multiprocessing`](https://docs.python.org/zh-cn/3/library/multiprocessing.html#module-multiprocessing) 包同时提供了本地和远程并发操作，通过使用子进程而非线程有效地绕过了 [全局解释器锁](https://docs.python.org/zh-cn/3/glossary.html#term-global-interpreter-lock)。 因此，[`multiprocessing`](https://docs.python.org/zh-cn/3/library/multiprocessing.html#module-multiprocessing) 模块允许程序员充分利用给定机器上的多个处理器。 它在 Unix 和 Windows 上均可运行。

一般使用方法：

```python
import os
import time
from multiprocessing import Process

start = time.time()
p1 = Process(target=my_sum1, args=(10000_0000,))
p2 = Process(target=my_sum2, args=(10000_0000,))
p3 = Process(target=my_sum3, args=(10000_0000,))
p1.start()
p2.start()
p3.start()
p1.join()
p2.join()
p3.join()
end = time.time()
print("总共用时 {} 秒".format((end - start)))
```

结果如下：

```bash
总共用时 4.6657445430755615 秒
```

不使用多进程时：

```python
def main(s):
    print(s.__name__)
    return s(10000_0000)
   
start = time.time()
main(my_sum1)
main(my_sum2)
main(my_sum3)
end = time.time()
print("总共用时 {} 秒".format((end - start)))
```

结果如下：

```python
my_sum1
my_sum2
my_sum3
总共用时 6.19011664390564 秒
```

可见多进程确实降低了时间。

当调用函数相同，参数变化时，可以使用如下方法：

```python
import time
from multiprocessing import Pool

start = time.time()
p = Pool(processes=os.cpu_count())
p.map_async(main, [my_sum1, my_sum2, my_sum3])
end = time.time()
print("总共用时 {} 秒".format((end - start)))
```

结果如下：

```bash
总共用时 0.3504462242126465 秒
my_sum1my_sum2

my_sum3
```

可以看出，结果比较混乱，而且计时不对，这是因为采用 map_async 异步非阻塞方式进行的。下面是非异步阻塞式：

```python
import time
from multiprocessing import Pool

start = time.time()
p = Pool(processes=os.cpu_count())
p.map(main, [my_sum1, my_sum2, my_sum3])
end = time.time()
print("总共用时 {} 秒".format((end - start)))
```

结果如下：

```bash
my_sum1my_sum2my_sum3


总共用时 5.382384300231934 秒
```

无论采用哪种多进程模式，都能够降低运行时间。

还有一种多进程使用方法：

```python
import os
from multiprocessing import Pool

import numpy as np

result_objs = []
start = time.time()
with Pool(processes=os.cpu_count()) as p:
    for f in [my_sum1, my_sum2, my_sum3]:
        result = p.apply_async(main, (f,))
        result_objs.append(result)
    # 使用get方法获取值
    results = [result.get() for result in result_objs]
    print(results)
end = time.time()
print("总共用时 {} 秒".format((end - start)))
```

结果如下：

```bash
my_sum2my_sum1my_sum3


[4999999950000000, 4999999950000000, 4999999950000000]
总共用时 5.462852716445923 秒
```

上面同样属于异步非阻塞方法，非异步方法如下：

```python
import os
from multiprocessing import Pool

import numpy as np

result_objs = []
start = time.time()
with Pool(processes=os.cpu_count()) as p:
    for f in [my_sum1, my_sum2, my_sum3]:
        result_objs.append(p.apply(main, (f,)))
end = time.time()
print("总共用时 {} 秒".format((end - start)))
print(result_objs)
```

结果如下：

```bash
my_sum1
my_sum2
my_sum3
总共用时 6.348566055297852 秒
[4999999950000000, 4999999950000000, 4999999950000000]
```

关于 map, map_async, apply, apply_async 的区别，如下表：

```markdown
             Multi-args   Concurrence    Blocking     Ordered-results
map          no           yes            yes          yes
apply        yes          no             yes          no
map_async    no           yes            no           yes
apply_async  yes          yes            no           no
```

可以根据需要选择相应的多进程方法。

关于异步非阻塞式和非异步阻塞式：

- 异步非阻塞式是主进程首先运行，碰到子进程不切换，当操作系统进行切换时，再运行子进程。如上面的 async 式的，主进程将首先遍历完整个代码，打印时间，切换后，运行子进程 p.map_async(main, [my_sum1, my_sum2, my_sum3]) 中的 main(my_sum1) 等。

  

- 非异步阻塞式是首先主进程开始运行，碰到子进程，操作系统切换到子进程，等待子进程运行结束后，再切换到另外一个子进程，直到所有子进程运行完毕。然后再切换到主进程，运行剩余的部分。

# 一个例子

求 2 ~ 250001 中所有素数的个数。

## 使用 C 语言编程

编程 C 程序，保存为 primes.c

```c
#include <stdio.h>
int isPrime(int n){
    for (int i=2; i<=n/2;i++){
        if (!(n%i)){
            return 0;
        }
    }
    return 1;
}

void main(){
    int numPrimes=0;
    for (int i=2; i<250001; i++){
        numPrimes+=isPrime(i);
    }
    printf("%d\n", numPrimes);
}
```

编译运行

```bash
# 编译
gcc primes.c -o primes
# 运行
time ./primes
```

结果如下，耗时 8秒多

```bash
$ time ./primes 
22044
./primes  8.52s user 0.00s system 99% cpu 8.522 total
```

下面，使用python程序测试运行时间。

## 编写第一个Python程序

编程 python 程序，保存为 primes.py

```python
def isPrime(n):
    """
    判断给定的n是否为素数
    """
    # 只需要用n除以2 ~ n/2 + 1 是否能整除
    # 如果有整数整除n，则n不是素数
    # 如果所有的都不能整数n，则n是素数
    for i in range(2, n//2 + 1):
        # 如果余数是0，则表示整除，不是素数，直接返回0
        if (not (n % i)):
            return 0
    # 如果所有的数都不能整除n，则说明n是素数，则返回1
    return 1

if __name__ == "__main__":
    numPrimes = 0  # 记录素数的个数
    
    # 统计2 ~ 250001 中有多少个素数
    for i in range(2, 250001):
        numPrimes += isPrime(i)
    # 打印素数的个数
    print(numPrimes)
```

不需要编译，直接运行。结果耗时111秒多

```bash
$ time python primes.py 
22044
python primes.py  111.71s user 0.02s system 99% cpu 1:51.76 total
```

## 编写第二个Python程序

使用numba加速。编程 python 程序，保存为 primesNumbaSpeed.py

```python
from numba import jit

@jit(nopython=True)
def isPrime(n):
    """
    判断给定的n是否为素数
    """
    # 只需要用n除以2 ~ n/2 + 1 是否能整除
    # 如果有整数整除n，则n不是素数
    # 如果所有的都不能整数n，则n是素数
    for i in range(2, n//2 + 1):
        # 如果余数是0，则表示整除，不是素数，直接返回0
        if (not (n % i)):
            return 0
    # 如果所有的数都不能整除n，则说明n是素数，则返回1
    return 1

if __name__ == "__main__":
    numPrimes = 0  # 记录素数的个数
    
    # 统计2 ~ 250001 中有多少个素数
    for i in range(2, 250001):
        numPrimes += isPrime(i)
    # 打印素数的个数
    print(numPrimes)
```

不需要编译，直接运行。结果耗时10秒多，比上一版快了10倍多

```bash
$ time python primesNumbaSpeed.py 
22044
python primesNumbaSpeed.py  10.52s user 3.39s system 140% cpu 9.929 total
```

## 编写第三个Python程序

使用numba 和 multiprocessing 加速。编程 python 程序，保存为 primesNumbaSpeed.py

```python
from multiprocessing import Pool

from numba import jit

def partition(l, size):
    """
    Returns a new list with elements
    of which is a list of certain size.
        >>> partition([1, 2, 3, 4], 3)
        [[1, 2, 3], [4]]
    """
    return [l[i : i + size] for i in range(0, len(l), size)]


@jit(nopython=True)
def isPrime(n):
    """
    判断给定的n是否为素数
    """
    # 只需要用n除以2 ~ n/2 + 1 是否能整除
    # 如果有整数整除n，则n不是素数
    # 如果所有的都不能整数n，则n是素数
    for i in range(2, n // 2 + 1):
        # 如果余数是0，则表示整除，不是素数，直接返回0
        if not (n % i):
            return 0
    # 如果所有的数都不能整除n，则说明n是素数，则返回1
    return 1


def sumPrime(l):
    return sum(list(map(isPrime, l)))


if __name__ == "__main__":
    numPrimes = 0  # 记录素数的个数
    lp = partition(range(2, 250001), 100)
    with Pool(15) as p:
        numPrimes = sum(p.map(sumPrime, lp))
    # 打印素数的个数
    print(numPrimes)
```

不需要编译，直接运行。显示耗时15 秒多。因为使用多进程，时间显示不准确，实际耗时只有1.4 秒左右。

```bash
$ time python primeMultiprocessSpeed.py
22044
python primeMultiprocessSpeed.py  15.33s user 9.42s system 1041% cpu 2.375 total
```

在 notebook 中调用打印真实时间

```python
from primeMultiprocessSpeed import sumPrime, partition
from multiprocessing import Pool
```

```python
%%timeit
numPrimes = 0  # 记录素数的个数
lp = partition(range(2, 250001), 100)
with Pool(15) as p:
    numPrimes = sum(p.map(sumPrime, lp))
# 打印素数的个数
print(numPrimes)
```

时间消耗 1.4 秒左右，比 C 语言写的代码还要快 3 倍。

```bash
22044
22044
22044
22044
22044
22044
22044
22044
1.4 s ± 85.3 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)
```

因此，当使用 python 进行快速开发时，如果需要优化运行速度，建议使用 numba 和 multiprocessing 进行加速。

# 使用 pyinstrument 发现 python 程序中执行耗时的部分

首先安装包

```
pip install pyinstrument
```

使用方法（在 notebook 中运行）

```python
!/usr/local/miniconda/bin/pyinstrument pyscripts/primeMultiprocessSpeed.py
```

结果如下

```markdown
22044

  _     ._   __/__   _ _  _  _ _/_   Recorded: 11:22:18  Samples:  592
 /_//_/// /_\ / //_// / //_'/ //     Duration: 2.449     CPU time: 2.772
/   _/                      v4.1.1

Program: pyscripts/primeMultiprocessSpeed.py

2.448 <module>  ../<string>:1
   [4 frames hidden]  .., runpy
      2.448 _run_code  runpy.py:64
      └─ 2.448 <module>  primeMultiprocessSpeed.py:1
         ├─ 1.639 map  multiprocessing/pool.py:359
         │     [7 frames hidden]  multiprocessing, threading, ..
         │        1.639 lock.acquire  ../<built-in>:0
         ├─ 0.703 <module>  numba/__init__.py:1
         │     [1389 frames hidden]  numba, llvmlite, pkg_resources, .., p...
         ├─ 0.037 Pool  multiprocessing/context.py:115
         │     [28 frames hidden]  multiprocessing, threading, ..
         ├─ 0.036 wrapper  numba/core/decorators.py:196
         │     [109 frames hidden]  numba, functools, llvmlite, ..
         └─ 0.025 __exit__  multiprocessing/pool.py:735
               [12 frames hidden]  multiprocessing, ..

To view this report with different options, run:
    pyinstrument --load-prev 2021-12-17T11-22-18 [options]

```

从每行的前面的运行时间，可以看到程序耗时的地方在哪里。

# 参考链接

1. [Python 进程池和线程池的简单使用](https://feelncut.com/2018/05/14/150.html)