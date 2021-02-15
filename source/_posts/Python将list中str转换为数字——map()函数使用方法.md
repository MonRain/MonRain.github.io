---
title: Python将list中str转换为数字 —— map()函数使用方法
categories: Python
tags:
- Python
- map()
- 日积月累
---


# Python将list中str转换为数字 —— map()函数使用方法
## 问题：list中str转换为数字
问题描述：若在数据处理过程中，遇到一个list变量

```
>>> a = ['2', '23', '43', '24']
```

其中的元素都为`str`类型，但数据处理需要数字类型，因此需要做进一步转化

我们利用迭代映射函数`map()`，先看一下`map()`函数的功能：

```
map(function, iterable, ...)
```

+ `function`, 对后边对象需要进行的操作
+ `iterable`, 希望被操作的对象

**注意**：`map()`函数在Python 2和Python 3中存在微小的差别：
+ Python 2返回操作后得到的list
+ Python 3返回迭代器

结合当前问题体会一下`map()`函数的操作

```
# Python 2
>>> a = ['2', '23', '43', '24']
>>> map(int,a)
[2, 23, 43, 24]
>>> map(float,a)
[2.0, 23.0, 43.0, 24.0]

# Python 3: 需要使用list将map()返回的迭代器转换为list对象
>>> a = ['2', '23', '43', '24']
>>> list(map(int,a))
[2, 23, 43, 24]
>>> list(map(float,a))
[2.0, 23.0, 43.0, 24.0]
```

相当于对`a`中每一个元素都进行了`int(x)`/`float(x)`操作

## map()函数的引申使用

我们可以利用`map()`进行更多自定义操作

```

>>> b=[1,2,3,4]

>>> def fun(x):
	return x+1

>>> list(map(fun,b))
[2, 3, 4, 5]
# 或使用lambda匿名函数
>>> list(map(lambda x: x+2,b))
[3, 4, 5, 6]

>>> c = [7,6,4,3]
>>> list(map(lambda x, y: x + 2 * y, b, c))
[15, 14, 11, 10]
```

**参考**

[Python map() 函数](https://www.runoob.com/python/python-func-map.html)
