---
title: 机器学习中的标准化和归一化方法
author: Jinzhong Xu
mathjax: true
date: 2020-05-08 11:30:46
tags:
- standardization
- normalization
categories:
- research
- machine learning
---

在机器学习实践中，经常会遇到对数据进行标准化、归一化等，那它与数学中的标准化、归一化、单位化有什么区别呢，python 包中如何实现这些标准化、归一化（或叫正则化）、单位化计算呢，还有哪些类似的计算方法呢，对于已知的数据集我该采用哪种标准化或归一化或单位化方法呢？这些问题一直是每一个机器学习从业者或学生们在实际工作中遇到的问题，而且网络上很多都介绍的不是很清晰，这里进行一个总结，以飨读者。

<!--more-->

# 标准化、归一化、单位化的区别

## 标准化 (Standardization)

首先看到标准化使我想起在概率论中对随机变量 $\xi$ 的标准化
$$
z = \frac{\xi - \mu}{\sigma}.
$$
这里，$\mu$ 是随机变量 $\xi$ 的均值，$\sigma$ 是标准差。它也叫做 $z$-score，多翻译为标准分数（standard score）。 经过变换后，随机变量 $z$ 就是标准差为 1 的随机变量，因此，也可以叫做标准差归一化。需要注意的是，因为标准差的计算公式是使 $\xi - \mu$ 和 $\sigma$ 具有相同的量纲，所以， $z$ 是一个没有量纲的随机变量，并且，可以证明随机变量 $\xi$ 和 $z$ 具有相同类型的分布。不过，通过 $z$-score 可以看出该值在分布中的相对位置，它指示了一个元素距离均值有几个标准偏差远。$z$-score 可正可负，也可以大于 1 或者小于 -1，不过所有 $z$-score均值为 0. 

NYU Stern Finance Professor Edward Altman 在1967年发明了 $z$-score ， 并在1968 年发布。在正态分布中，68% 的 $z$-score 落在 [-1, 1] 中，95% 的 $z$-score 落在 [-2, 2] 中，99% 的 $z$-score 落在 [-3, 3] 中。

## 正则化或归一化 (Normalization)

归一化，单从字面意思可以理解为将数据缩放或映射到区间 [0, 1] 内。其实，现在已经对其进行了推广，可以不仅仅限制的正数范围内，可以映射到区间 [-1, 1] 中。在机器学习中，常用的归一化方法有两种，公式表示分别是
$$
y_{\min-\max} = \frac{x - \min(x)}{\max(x) - \min(x)},
$$
$$
y_{\text{mean}} = \frac{x - \text{mean}(x)}{\max(x) - \min(x)}.
$$



分别称为 $\min - \max$ 归一化和 $\text{mean}$ 归一化。可以看出，经过这两种归一化后同样能够消除量纲对计算的影响。前者归一化后的结果都是正数，后者可正可负。可以根据需要进行选择。前者的  $\min - \max$ 归一化也可以看作是最大值归一化或者最小值归零化。后者的 $\text{mean}$ 归一化其实是均值归零化，但是映射后所有的量的绝对值都限制在 0-1之间。

## 单位化 (Unitization)

这个概念在机器学习中虽然不常出现，但是，它的含义已经在机器学习中显露。最早接触单位化是在数学中向量的单位化，即将一个向量不改变其方向只对其长度进行缩放到单位长度 1. 在机器学习中，如果把某一个特征的所有观测组成一个向量，再进行单位化，那么类似于前面的归一化方法，将会把所有值映射为 $[-1, 1]$ 中。数学上单位化的公式如下
$$
\vec{e} = \frac{\vec{x}}{\parallel \vec{x} \parallel _2}.
$$


这里，计算的是二范数 $l_2$，在机器学习中，也有定义为其他函数的，如 $l_1$, $l_{\infty}$等。

## 三者的本质

从数学上看，它们的计算公式可以用通用形式表示如下
$$
y = f(x) = \frac{x - A}{B}.
$$
就是将变量 $x$ 先进行平移 $A$ 个单位，然后再缩放到原来的 $\frac{1}{B}$. 在机器学习或概率论中为了计算和分析的方便，一般选择 $B$ 和变量具有相同量纲，这样可以消去量纲对后面计算的影响，$A$ 的选择将会影响映射后变量取值的正负等。

# Python 中标准化、归一化的实现

## 标准化

标准化方法在 Python 中有包 scikit-learn 的 preprocessing 模块，实现函数是 Scale 和 StandardScaler.

使用这种缩放的动机包括使得缩放后的值保留原数据集的分布信息。

### scale  ($z$-score)

函数 scale 提供了一种快速简单的方法来对单个类似数组的数据集执行标准化操作：

```python
from sklearn.preprocessing import scale
import numpy as np
X_train = np.array([[ 1., -1.,  2.], 
                    [ 2.,  0.,  0.], 
                    [ 0.,  1., -1.]])
X_scaled = scale(X_train)
X_scaled
```

```mathematica
array([[ 0.        , -1.22474487,  1.33630621],
       [ 1.22474487,  0.        , -0.26726124],
       [-1.22474487,  1.22474487, -1.06904497]])
```

### StandardScaler ($z$-score)

程序类 StandardScaler 实现了 Transformer API，以计算训练集上的均值和标准差，以便以后能够在测试集上重新应用相同的变换。因此，此类适合在 sklearn.pipeline.Pipeline 的早期步骤中使用：

```python
from sklearn.preprocessing import StandardScaler
data = [[0, 0], [0, 0], [1, 1], [1, 1]]
scaler = StandardScaler()
scaler.fit(data)  # 先使用 data 训练，即计算均值和标准差
scaler.transform([[0.5, 0.5]])  # 使用上面计算的均值和标准差计算 z-scores
```

```mathematica
array([[0., 0.]])
```

也可以直接计算 data 的 $z$-score

```python
scaler.fit_transform(data)
```

```mathematica
array([[-1., -1.],
       [-1., -1.],
       [ 1.,  1.],
       [ 1.,  1.]])
```

### RobustScaler and robust_scale (approx $z$-score)

标准化计算容易受到异常值的影响，因此 Python 包 scikit-learn 包的模块 preprocessing 中提供了函数 RobustScaler 和它的便携计算 roubust_scale. 在 RobustScaler 中的 **quantile_range** 中默认 **(q_min, q_max) = （25.0, 75.0）** ，即 (1st quantile, 3rd quantile) = IQR Quantile ，使用 1 分位数 和 3 分位数里的数值计算均值和方差用于变化其他数据。

更多具体的参数请参考官网介绍

- [sklearn.preprocessing.scale](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.scale.html)
- [sklearn.preprocessing.StandardScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)
- [sklearn.preprocessing.RobustScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html#sklearn.preprocessing.RobustScaler)
- [sklearn.preprocessing.robust_scale](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.robust_scale.html#sklearn.preprocessing.robust_scale)

## 归一化

归一化方法在 Python 中有包 scikit-learn 中的 preprocessing 模块，实现函数是 MinMaxScaler 和 minmax_scale.

使用这种缩放的动机包括对特征的非常小的标准偏差具有鲁棒性增强稳定性，并在稀疏数据中保留 0 值。

### minmax_scale (min - max 归一化)

函数 minmax_scale 提供了一种快速简单的方法来对单个类似数组的数据集执行 min-max 归一化操作：

```python
from sklearn.preprocessing import minmax_scale
import numpy as np
X_train = np.array([[ 1., -1.,  2.], 
                    [ 2.,  0.,  0.], 
                    [ 0.,  1., -1.]])
X_scaled = scale(X_train)
X_scaled
```

```mathematica
array([[0.5       , 0.        , 1.        ],
       [1.        , 0.5       , 0.33333333],
       [0.        , 1.        , 0.        ]])
```

### MinMaxScaler  (min - max 归一化)

默认 MinMaxScaler 将数据映射到 $[0, 1]$ 中，也可以修改参数 scaler = MinMaxScaler(feature_range=(-5, 5)) 将 data 映射到自己需要的区间，如这里的 $[-5, 5]$. 内部计算公式如下

```mathematica
X_std = (X - X.min(axis=0)) / (X.max(axis=0) - X.min(axis=0))  # min, max 为 data 的最值
X_scaled = X_std * (max - min) + min  # min, max 为 feature_range 中设置的区间端点
```

```python
from sklearn.preprocessing import MinMaxScaler
data = [[-1, 2], [-0.5, 6], [0, 10], [1, 18]]
scaler = MinMaxScaler()
scaler.fit(data)
scaler.transform([[0, 0]])
```

```mathematica
array([[ 0.5  , -0.125]])
```

也可以直接计算 data 的 min - max 归一化值

```python
scaler.fit_transform(data)
```

```mathematica
array([[0.  , 0.  ],
       [0.25, 0.25],
       [0.5 , 0.5 ],
       [1.  , 1.  ]])
```

更多具体的参数请参考官网介绍

- [sklearn.preprocessing.MinMaxScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html#sklearn.preprocessing.MinMaxScaler)
- [sklearn.preprocessing.minmax_scale](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.minmax_scale.html#sklearn.preprocessing.minmax_scale)

## 单位化 (vector unitization)

单位化方法在 Python 中有包 scikit-learn 中的 preprocessing 模块，实现函数是 MaxAbsScaler 和 maxabs_scale （ 注意这里的分母是 $l_{\infty}$ ），以及 Normalizer 和 normalize (分母可以自己指定)。

类似于 min - max 归一化的 MinMaxScaler ，使用这种缩放的动机包括对特征的非常小的标准偏差具有鲁棒性增强稳定性，并在稀疏数据中保留 0 值。

### maxabs_scale (vector unitization)

函数 maxabs_scale 提供了一种快速简单的方法来对单个类似数组的数据集执行单位化操作：

```python
from sklearn.preprocessing import maxabs_scale
import numpy as np
X_train = np.array([[ 1., -1.,  2.], 
                    [ 2.,  0.,  0.], 
                    [ 0.,  1., -1.]])
X_scaled = maxabs_scale(X_train)
X_scaled
```

```mathematica
array([[ 0.5, -1. ,  1. ],
       [ 1. ,  0. ,  0. ],
       [ 0. ,  1. , -0.5]])
```



### MaxAbsScaler (vector unitization)

MaxAbsScaler 将数据缩放到区间 $[-1, 1]$，并且消除了量纲的影响为以后数据的进一步处理。

```python
from sklearn.preprocessing import MaxAbsScaler
X = [[ 1., -1.,  2.], 
     [ 2.,  0.,  0.], 
     [ 0.,  1., -1.]]
scaler = MaxAbsScaler()
scaler.fit(X)
scaler.transform([[0, 0.5, 0.5]])
```

```mathematica
array([[0.  , 0.5 , 0.25]])
```

也可以直接计算 X 的 单位化， 按照每列

```python
scaler.fit_transform(X)
```

```mathematica
array([[ 0.5, -1. ,  1. ],
       [ 1. ,  0. ,  0. ],
       [ 0. ,  1. , -0.5]])
```

### normalize

```python
from sklearn.preprocessing import normalize
import numpy as np
X_train = np.array([[ 1., -1.,  2.], 
                    [ 2.,  0.,  0.], 
                    [ 0.,  1., -1.]])
X_scaled = normalize(X_train, norm='l2')
X_scaled
```

```mathematica
array([[ 0.40824829, -0.40824829,  0.81649658],
       [ 1.        ,  0.        ,  0.        ],
       [ 0.        ,  0.70710678, -0.70710678]])
```

### Normalizer

可以自己指定范数 **‘l1’, ‘l2’, or ‘max’, optional (‘l2’ by default)**

```python
from sklearn.preprocessing import Normalizer
X = [[ 1., -1.,  2.], 
     [ 2.,  0.,  0.], 
     [ 0.,  1., -1.]]
scaler = Normalizer(norm='l2')
scaler.fit(X)
scaler.transform([[0, 0.5, 0.5]])
```

```mathematica
array([[0.        , 0.70710678, 0.70710678]])
```

```python
scaler.fit_transform(X)
```

```mathematica
array([[ 0.40824829, -0.40824829,  0.81649658],
       [ 1.        ,  0.        ,  0.        ],
       [ 0.        ,  0.70710678, -0.70710678]])
```

更多具体的参数请参考官网介绍

- [sklearn.preprocessing.MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler)
- [sklearn.preprocessing.maxabs_scale](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.maxabs_scale.html#sklearn.preprocessing.maxabs_scale)
- [sklearn.preprocessing.Normalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html#sklearn.preprocessing.Normalizer)
- [sklearn.preprocessing.normalize](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.normalize.html#sklearn.preprocessing.normalize)

# Python 中 scikit-learn 包提供的其他数据变换或预处理方法

## 编码类别特征

### OrdinalEncoder (categorical to integer)

```python
from sklearn.preprocessing import OrdinalEncoder
enc = OrdinalEncoder()
X = [['male', 'from US', 'uses Safari'], ['female', 'from Europe', 'uses Firefox']]
enc.fit(X)
enc.transform([['female', 'from US', 'uses Safari']])
```

```mathematica
array([[0., 1., 1.]])
```

### OneHotEncoder (categorical to vector)

```python
from sklearn.preprocessing import OneHotEncoder
enc = OneHotEncoder()
X = [['male', 'from US', 'uses Safari'], ['female', 'from Europe', 'uses Firefox']]
enc.fit(X)
enc.transform([['female', 'from US', 'uses Safari'],  ['male', 'from Europe', 'uses Safari']]).toarray()
```

```mathematica
array([[1., 0., 0., 1., 0., 1.],
       [0., 1., 1., 0., 0., 1.]])
```

```python
# Encoding categorical features : OneHotEncoder : 自己指定类别

from sklearn.preprocessing import OneHotEncoder
genders = ['female', 'male']
locations = ['from Africa', 'from Asia', 'from Europe', 'from US']
browsers = ['uses Chrome', 'uses Firefox', 'uses IE', 'uses Safari']
enc = OneHotEncoder(categories=[genders, locations, browsers])
X = [['male', 'from US', 'uses Safari'], ['female', 'from Europe', 'uses Firefox']]
enc.fit(X)
enc.transform([['female', 'from Asia', 'uses Chrome']]).toarray()
```

```mathematica
array([[1., 0., 0., 1., 0., 0., 1., 0., 0., 0.]])
```

```python
# Encoding categorical features : OneHotEncoder : 忽略未知类别

from sklearn.preprocessing import OneHotEncoder
env = OneHotEncoder(handle_unknown='ignore')
X = [['male', 'from US', 'uses Safari'], ['female', 'from Europe', 'uses Firefox']]
enc.fit(X)
enc.transform([['female', 'from Asia', 'uses Chrome']]).toarray()
```

```mathematica
array([[1., 0., 0., 1., 0., 0., 1., 0., 0., 0.]])
```

```python
# Encoding categorical features : OneHotEncoder : 忽略未知类别
from sklearn.preprocessing import OneHotEncoder
env = OneHotEncoder(drop='first')
X = [['male', 'from US', 'uses Safari'], ['female', 'from Europe', 'uses Firefox']]
env.fit(X)
env.transform(X).toarray()
```

```mathematica
array([[1., 1., 1.],
       [0., 0., 0.]])
```

## 映射到某种分布上

### Mapping to a Uniform distribution (QuantileTransformer or quantile_transform)

### Mapping to a Gaussian distribution (PowerTransformer)

## 连续数据到离散数据

### KBinDiscretizer (continuous to bin)

```python
# Discretization :  K-bins discretization

from sklearn.preprocessing import KBinsDiscretizer
import numpy as np

est = KBinsDiscretizer(n_bins=[3, 2, 2], encode='ordinal')
X =  np.array([[ -3., 5., 15 ], [  0., 6., 14 ], [  6., 3., 11 ]])
est.fit(X)
est.transform(X)
```

```mathematica
array([[0., 1., 1.],
       [1., 1., 1.],
       [2., 0., 0.]])
```

### Binarizer (continuous to boolean)

```python
# Discretization : Binarizer
from sklearn.preprocessing import Binarizer
X = [[ 1., -1.,  2.], [ 2.,  0.,  0.], [ 0.,  1., -1.]]
binarizer = Binarizer(threshold=1.1)
binarizer.transform(X)
```

```mathematica
array([[0., 0., 1.],
       [1., 0., 0.],
       [0., 0., 0.]])
```

### PolynomialFeatures (add features)

## 自定义变换

### log1p

```python
# Custom transformer

import numpy as np
from sklearn.preprocessing import FunctionTransformer
transformer = FunctionTransformer(np.log1p, validate=True)
X = np.array([[0, 1], [2, 3]])
transformer.transform(X)
```

```mathematica
array([[0.        , 0.69314718],
       [1.09861229, 1.38629436]])
```

更多请参考：

1. Python 中 scikit-learn 提供的 标准化，归一化和单位化方法介绍 [Preprocessing data](https://scikit-learn.org/stable/modules/preprocessing.html#preprocessing-scaler)
2. Python 中各中数据预处理缩放方法对比图 [Compare the effect of different scalers on data with outliers](https://scikit-learn.org/stable/auto_examples/preprocessing/plot_all_scaling.html#sphx-glr-auto-examples-preprocessing-plot-all-scaling-py)