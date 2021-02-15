---
title: Python日积月累
categories: Python
tags:
- Python
- 日积月累
---


# python 判断元素是否包含于list

这里涉及到list的查找方法，参考[python中list的四种查找方法](https://blog.csdn.net/lachesis999/article/details/53185299)

```Python
>>> a = [2,1,3,2,6]
>>> 2 in a
True
>>> 9 in a
False
>>> 2 not in a
False
>>> 9 not in a
True
```


# Python 拟合正态分布

**待解决的问题**：对一个一维的数串拟合到正态分布。
所谓拟合正态分布，实质上算出均值$\mu$和方差$\sigma^2$即可确定分布。

```Python
>>> X = [1,2,3,34,3,23,56,7,87,6,5,34,5678,7,654,3,45,67,8,3,2]
>>> mu = np.mean(X)
>>> sigma = np.std(X)
>>> mu,sigma**2
(320.3809523809524, 1453914.5215419505)
```

# Python 中三元表达式的实现

C语言等其它高级语言中，都有`?:`这样的三元表达式替换复杂的`if else`。

在`python`中，本身没有三元表达式，但可以利用下面的简单语句实现相似功能。

```Python
A if a>b else B
# 等价于
if a>b:
    return A
else:
    return B
```

或

```Python
(B ,A )[a>b] # (假则, 真则)[判断]
# 等价于
if a>b:
    return A
else:
    return B
```

但第二种方法比较不受程序员们的待见，实质上是利用了将`False`等同于`0`，将`True`等同于`1`，然后对前边的`tuple`进行索引。

**参考**
[1]. [python 中 ? : 三元表达式 的实现方式](https://blog.csdn.net/Sinchb/article/details/8081754)
[1]. [三元运算符- Python进阶](https://eastlakeside.gitbook.io/interpy-zh/ternary_operators)
[3]. [python中的三元表达式（三目运算符）](https://www.cnblogs.com/mywood/p/7416893.html)