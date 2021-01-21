---
title: numpy对数据集的基本操作：归一化、最值、均值、读取与保存
categories: Python
tags:
- numpy
---

# numpy对数据集的基本操作：归一化、最值、均值、读取与保存

```
import numpy as np
```

## 一、 numpy对数据集的存取

```
# 当前工作目录
pwd: ~

origin_dataset: ~/example.csv
```

最正宗的展示格式应该是txt，但csv格式文件只不过是用分隔符分开的数据组成的简单文本文件，因此这里用csv也无伤大雅。而且许多数据集也都是csv格式。

1. **读取**csv到变量：loadtxt()

```
np.loadtxt(filepath, delimiter, usecols, unpack, ...)
```

+ **filepath**:加载文件路径
+ **delimiter**:加载文件分隔符
+ **usecols**:加载数据文件中列索引,**人话**：*指定加载数据中的第几列，默认全加载*
+ **unpack**:当加载多列数据时是否需要将数据列进行解耦赋值给不同的变量

```
>>> data_path = "~/example.csv"
>>> data = np.loadtxt(data_path, delimiter=',')

>>> data
([...]
    [...]
    ...
    [...])
```

2. **保存**变量到文件：savetxt()

```
np.savetxt(fileName, data, ...)
```

+ **fileName**:保存文件路径和名称
+ **data**:需要保存的数据

```
data_path = "~/example.csv"
np.savetxt("saveddata.csv", data)
```

## 二、numpy获取最值、均值

测试数据
```
>>> a=[[1,2,3],[2,5,7],[6,3,6],[7,3,2]]
>>> x=np.asanyarray(a)
>>> x
array([[1, 2, 3],
       [2, 5, 7],
       [6, 3, 6],
       [7, 3, 2]])
```
numpy集成了获取数据最值、均值甚至更多操作的方法，这一点用起来和matlab很像

```
np.max(data, axis, ...)
np.min(data, axis, ...)
np.mean(data, axis, ...)
```


需要注意的是最常用的`axis`选项，对于2维数组（矩阵）而言：

+ **axis = None**: （默认）返回所有数据（行、列）中的最值/均值，返回一个数
+ **axis = 0**: 返回所有列的最值/均值，相当于返回一个行向量（元素数与**列数**相同的array）
+ **axis = 1**: 返回所有行的最值/均值，相当于返回一个列向量（元素数与**行数**相同的array）

```
>>> np.max(x)
7

>>> np.max(x,axis=0)
array([7, 5, 7])

>>> np.max(x,axis=1)
array([3, 7, 6, 7])

>>> np.min(x)
1

>>> np.min(x,axis=0)
array([1, 2, 2])

>>> np.min(x,axis=1)
array([1, 2, 3, 2])

>>> np.mean(x)
3.9166666666666665

>>> np.mean(x,axis=0)
array([4.  , 3.25, 4.5 ])

>>> np.mean(x,axis=1)
array([2.        , 4.66666667, 5.        , 4.        ])
```

## 三、numpy对数据进行归一化

以下是笔者处理KDD99数据集时归一化的小demo，也是在此途中遇到的上述问题，记录下来，供自己以后参考。

```
print('normlizing...')
kdd99_numeric = np.loadtxt(numeric_datasets, delimiter=',')
mean = np.mean(kdd99_numeric, axis=0)
max = np.max(kdd99_numeric, axis=0) + 1e-8
min = np.min(kdd99_numeric, axis=0)
kdd99_numeric_norm = (kdd99_numeric - mean) / (max - min)
print('Done')
print(kdd99_numeric_norm)

np.savetxt(norm_numeric_datasets, kdd99_numeric_norm, delimiter=',')
```

上述使用的是所谓 **min-max标准化（Min-Max Normalization）（线性函数归一化）** 方法，除此之外，在笔者还了解到还存在 **Z-score标准化方法** ，关于此信息可以查阅参考资料[2]。

## 参考资料
[1]. [Numpy读取csv文件](https://blog.csdn.net/MESSI_JAMES/article/details/80487389)
[2]. [机器学习-数据归一化方法（Normalization Method）](https://blog.csdn.net/program_developer/article/details/78637711)