---
title: 机器学习优化算法：从SGD到Adam
author: Jinzhong Xu
mathjax: true
date: 2020-05-30 13:44:47
tags:
- optimizer
- ml
categories:
- research
- machine learning
---

机器学习是实现人工智能的一种有效方法，当构建好模型后，需要使用优化器迭代学习模型参数，常用的有随机梯度下降法（SGD）和 Adam，下面总结一下各种的优缺点，并介绍一点优化器的演变过程。

<!--more-->

# 随机梯度下降法 SGD

SGD 是一种梯度下降算法，为什么叫做随机梯度下降呢，因为每次迭代都是随机选择一个样本，计算损失函数沿着负梯度方向求极小值，而梯度下降算法是将所有样本计算损失函数的平均，然后再计算损失函数沿着负梯度方向求极小值，通过链式法则一层一层依据梯度下降方向来更新模型参数。

SGD 的计算公式如下（与梯度下降算法相同）：
$$
\Delta \theta_t = - \alpha \times g_t.
$$
这里，$\alpha$ 是学习率或步长，$g_t$ 是梯度方向，$\Delta \theta_t$ 是损失函数优化量，包含了优化迭代的方向和步长。优化变量公式如下：
$$
x = x + \Delta \theta_t.
$$


SGD 的缺点如下：

1. 容易陷入局部极值（这是梯度下降算法的通病，因为复杂模型的损失函数一般都不是凸函数，具有多局部极值）
2. 遇到鞍点，模型无法更新参数
3. 受初始学习率（最优化理论中的步长）的影响很大
4. 每个维度上学习率都一样（特别是在稀疏数据下出现问题）

如何解决这些问题，特别是随着迭代的增加，能够动态的调整学习率，遇到鞍点时能够不仅仅依靠当前点的梯度而是增加动量冲过鞍点，下面一些方法是对SGD的改进。

# AdaGrad

AdaGrad 算法的计算公式如下：
$$
n_t = n_{t-1} + g^2_t, \\\\
\Delta \theta_t = -\frac{\alpha}{\sqrt{n_t + \epsilon}} \times g_t.
$$
其中，$g_t$ 是当前梯度方向，$n_t$ 是梯度平方的积累值，$\frac{\alpha}{\sqrt{n_t + \epsilon}}$是学习率（可以看出随着优化迭代的进行，学习率递减，这里$\epsilon$ 是一个小正数，防止初始梯度$n_t=0$）。AdaGrad 算法没有改变SGD算法优化的方向，只是更改了每次迭代的学习率或步长，使得每次迭代都迈更小的步，特别是在极小点附近，这将是有益的。

伪代码如下：

```python
grad_squard = 0
while True:
	dx = compute_gradient(x)
    grad_squard += dx * dx
    x -= learning_rate * dx / (np.sqrt(grad_squard + 1e-7))
```

AdaGrad 算法的优点：

1. 前期放大学习率，因前期梯度累积值较小
2. 后期缩小学习率，因后期梯度累计值较小
3. 学习率随着训练次数降低
4. 每个分量有不同的学习率，这是因为每个分量的梯度方向不同导致，累积的梯度平方和不同

AdaGrad 算法的缺点：

1. 学习率设置太大时，会导致优化迭代比较敏感，不收敛
2. 后期因梯度评分累积值太大，使得学习率较小，即步长小，导致基本上不走不优化，提前结束训练

# RMSProp

RMSProp 是解决AdaGrad的学习率后期太小而提出，主要是解决$\sqrt{n_t + \epsilon}$ 梯度平方和累积过大问题，考虑求当前梯度和累积梯度的加权平均。

RMSProp 算法计算公式如下：
$$
n_t = \gamma n_{t-1} + (1-\gamma)g^2_t, \\\\
\Delta \theta_t = -\frac{\alpha}{\sqrt{n_t + \epsilon}}\times g_t
$$
伪代码如下：

```python
grad_squard = 0
while True:
	dx = compute_gradient(x)
    grad_squard = decay_rate * grad_squard + (1 - decay_rate) * dx * dx
    x -= learning_rate * dx / (np.sqrt(grad_squard + 1e-7))
```

RMSProp 算法的优点：

1. RMSProp 解决AdaGrad 训练后期提前结束的问题（学习率太小不更新）

RMSProp 算法的缺点：

1. 只解决了学习率更新问题，为解决梯度方向问题，遇到鞍点时无法更新

# Adam

Adam 算法的计算公式如下：
$$
m_t = \beta \times m_{t-1} + (1-\beta) \times g_t, \\\\
n_t = \gamma \times n_{t-q} + (1-\gamma) \times g^2_t, \\\\
\Delta \theta_t = - \frac{\alpha}{\sqrt{n_t} + \epsilon} \times m_t.
$$

这里，$m_t$ 是为梯度添加累积动量的加权平均梯度方向，$n_t$ 是为学习率的累积梯度平方缩放率。

Adam 算法的伪代码如下：

```python
first_moment = 0
second_moment = 0
while True:
	dx = compute_gradient(x)
	first_moment = beta1 * first_moment + (1 - beta1) * dx
	second_moment = beta2 * second_moment + (1 - beta2) * dx * dx
	x -= first_moment * learning_rate / (np.sqrt(second_moment) + 1e-7)
```

Adam 算法的优点：

1. 学习率按照RMSProp算法更新，能够保证每步迭代学习率都在减小，而且每个维度不一样，且不会像AdaGrad算法提前结束
2. 梯度方向提升为动量和梯度的加权平均，能够有效逃避鞍点问题

更进一步的，Adam算法引入校准值，伪代码如下：

```python
first_moment = 0
second_moment = 0
for t in range(1, num_iterations):
	dx = compute_gradient(x)
	first_moment = beta1 * first_moment + (1 - beta1) * dx
	second_moment = beta2 * second_moment + (1 - beta2) * dx * dx
	first_unbias = first_moment / (1 - beta1 ** t)
	second_unbias = second_moment / (1 - beta2 ** t)
	x -= first_unbias * learning_rate / (np.sqrt(second_unbias) + 1e-7)
```

# ALL IN ALL

无论是哪种优化算法，都需要初始化一个学习率，训练模型时，推荐使用SGD和Adam算法都尝试一下。SGD 通常训练时间会更长，但最终效果比较好，当然需要好的初始化和学习率。当需要训练较深较复杂的网络且需要快速收敛时，推荐使用Adam. 一般 Adam 算法的参数可选择：

1. ```html
   1. beta1 = 0.9
   2. beta2 = 0.999
   3. learning_rate = 1e-3 or 5e - 4
   ```

   

其他，学习率自适应方法，如

1. 指数衰减 $\alpha = \alpha_0 e^{-kt}$.
2. 倒数衰减 $\alpha = \alpha_p / (1 + kt)$.