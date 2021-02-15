---
title: Python获取list特定元素下标
categories: Python
tags:
- Python
- list查找
- 日积月累
---


# python获取list特定元素下标
反索引，一般情况我们通过索引获取list中的元素。
如：

```
>>> a = [2,1,3,4,6]
>>> a[3]
4
```

有些时候，我们已知list中的某些元素，但需要通过已知元素获得其在list中的索引位置。

## 方法1：利用builtin方法item.index(list)

```
>>> a = [2,1,3,4,6]
>>> a.index(3)
2
```

**注意** 如果list中存在重复值，这个方法只能获得第一个值的index。

```
>>> a = [2,1,3,2,6]
>>> a.index(2)
0
```

## 方法2：enumerate()方法

```
>>> a = [2,1,3,2,6]
>>> e=enumerate(a)
>>> e
<enumerate object at 0x000001A6FF29D7C0>
>>> list(e)
[(0, 2), (1, 1), (2, 3), (3, 2), (4, 6)]
```

不难发现，`enumerate()`方法包含的值的形式为

```
(index 0, value 0),(index 1, value 1),(index 2, value 2)...
```

可以通过利用这个信息获得元素的index

```
>>> print([i for i,x in enumerate(a) if x==2])
[0, 3]
```

**参考**
[python 获取list特定元素下标](https://blog.csdn.net/qq_24737639/article/details/78839678)
