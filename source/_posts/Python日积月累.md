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
所谓拟合正态分布，实质上算出均值 $ \mu $ 和方差 $ \sigma^2 $ 即可确定分布。

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

# 进度条工具TQDM

[GitHub项目地址](https://github.com/tqdm/tqdm)
[文档地址](https://tqdm.github.io/)

看上去很酷炫的进度条工具，可以可视化**time consuming**的程序等待过程。

使用方法在`PySAD`的[Example](https://pysad.readthedocs.io/en/latest/examples.html#example-full-usage)中有体现。

![项目仓库中的示例](https://gitee.com/songz7026/image-pool/raw/master/Python/tqdm%E6%BC%94%E7%A4%BA.gif)

# Python删除变量

在Python中删除指定变量以节省空间，使用`del var`命令

```Python
>>> a=1
>>> a
1
>>> del a
>>> a
Traceback (most recent call last):
  File "<input>", line 1, in <module>
NameError: name 'a' is not defined

```

# 关于Python中Iterator重置问题

Iterator是Python中常见的类型，不特指具体类型。

一般此类只提供__next__()方法获得迭代器中下一个元素。如：

```Python
>>> a=np.random.random((3,4))
>>> b=iter(a)    
>>> a
array([[0.73612967, 0.01244284, 0.21440946, 0.29223116],
       [0.39542505, 0.14566755, 0.21281517, 0.04957406],
       [0.90193673, 0.1955091 , 0.64111833, 0.78027169]])
>>> b.__next__()
array([0.73612967, 0.01244284, 0.21440946, 0.29223116])
>>> b.__next__()
array([0.39542505, 0.14566755, 0.21281517, 0.04957406])
>>> b.__iter__()
<iterator object at 0x000001BFA787FF08>
```

**问题**：有的时候需要**重新进行迭代**，即对迭代器的指针重置到初始位置。

但是迭代器一般只有`iter()`和`next()`方法，没有重置的方法，非常的令人头疼。

经过调查，有关于**tee**的方法，但是存在质疑，因为它主要不是干这个的。所以目前笔者能够想到的方法是：**重新定义一个新的迭代器**。

```Python
>>> a=np.random.random((3,4))
>>> b=iter(a)    
>>> a
array([[0.73612967, 0.01244284, 0.21440946, 0.29223116],
       [0.39542505, 0.14566755, 0.21281517, 0.04957406],
       [0.90193673, 0.1955091 , 0.64111833, 0.78027169]])
>>> b.__next__()
array([0.73612967, 0.01244284, 0.21440946, 0.29223116])
>>> b.__next__()
array([0.39542505, 0.14566755, 0.21281517, 0.04957406])
>>> b.__iter__()
<iterator object at 0x000001BFA787FF08>
>>> b.__next__()
array([0.90193673, 0.1955091 , 0.64111833, 0.78027169])
>>> b.__next__()
Traceback (most recent call last):
  File "<input>", line 1, in <module>
StopIteration
>>> c=iter(a)
>>> c.__next__()
array([0.73612967, 0.01244284, 0.21440946, 0.29223116])
>>> c.__next__()
array([0.39542505, 0.14566755, 0.21281517, 0.04957406])
>>> c.__next__()
array([0.90193673, 0.1955091 , 0.64111833, 0.78027169])
>>> c.__next__()
Traceback (most recent call last):
  File "<input>", line 1, in <module>
StopIteration
```

新的小tip：可以删除原来的迭代器，重新定义重名变量，节省空间。

```Python
>>> del b
>>> b.__next__()
Traceback (most recent call last):
  File "<input>", line 1, in <module>
NameError: name 'b' is not defined
>>> b=iter(a)
>>> b.__next__()
array([0.73612967, 0.01244284, 0.21440946, 0.29223116])
```

# 使用pandas查找数组中元素出现的次数


```Python
>>> import pandas as pd
>>> a=np.random.random((3,4))
>>> a=[1,2,2,3,4,4]
>>> pd.value_counts(a)
# 元素 出现次数 
2    2
4    2
1    1
3    1
dtype: int64
>>> np.array(pd.value_counts(a))
array([2, 2, 1, 1], dtype=int64)
```

# List的中是否包含某元素

```Python
>>> a=[1,2,2,3,4,4]
>>> a.__contains__(2)
True
>>> a.__contains__(6)
False

```

