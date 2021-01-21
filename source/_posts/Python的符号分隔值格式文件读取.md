---
title: Python的符号分隔值格式(xsv)文件读取
categories: Python
tags:
- python
- 文件读取(File I/O)
- 迭代器
- csv文件
- with()
- open()
---

# Python的符号分隔值格式(xsv)文件读取

## 一、xsv文件介绍

xsv(x-separated values)x分隔值文件格式，最常见的是

> csv(Comma-Separated Values, 逗号分隔值)
> tsv(Tab-separated values, 制表符分隔值)
> <font size=2, color='gray'> xsv 名称是笔者杜撰，不确定是否真的有这个叫法，也不确定除了上述两种格式是否还有其它格式，即其它分隔方式。这样做的目的是将此类文件统称起来。 </font>

一定意义上来说，这类文件格式的区别其实就是分隔符不同，在python中体现为`delimiter`等参数不同。

此类文件用简单的**文件编辑器**即可打开，如 *Windows系统自带的记事本(nptepad)、关爱孤儿的vim编辑器* 以及 *VSCode* 等。不过一般此类文件用于存储大批量有规则的数据，主要使用`code`进行批量自动化读取和更多操作。

以下是来自wikipida的解释：

### 1、csv格式

制表符分隔值 （Tab-separated values，TSV）格式文件是一种用于储存数据的文本格式文件，其数据以表格结构（例如 数据库 或 电子表格 数据）储存。每一行储存一条记录。 每条记录的各个字段间以制表符作为分隔。

TSV 格式是一种被广泛支持的文件格式，它经常用来在不同的计算机程序之间传递数据，支持格式。 例如，TSV文件可以用来在数据库和电子表格之间传递数据。

TSV 格式是逗号分隔值（CSV）格式的一种变体，CSV 格式以逗号作为字段间的分隔符号，因为逗号本身是一种很常见的文本数据，因此常常会引起一些问题，而制表符在文本数据中相对少见。在 IANA 标准中，数据字段内禁止使用制表符。
### 2、tsv格式

逗号分隔值（Comma-Separated Values，CSV，有时也称为字符分隔值，因为分隔字符也可以不是逗号），其文件以纯文本形式存储表格数据（数字和文本）。纯文本意味着该文件是一个字符序列，不含必须像二进制数字那样被解读的数据。CSV文件由任意数目的记录组成，记录间以某种换行符分隔；每条记录由字段组成，字段间的分隔符是其它字符或字符串，最常见的是逗号或制表符。通常，所有记录都有完全相同的字段序列。

CSV文件格式的通用标准并不存在，但是在RFC 4180中有基础性的描述。使用的字符编码同样没有被指定，但是7-bit ASCII是最基本的通用编码。

+ 由于笔者是通过关注到tshark解析pcap文件的代码了解到tsv文件的，因此以下记录了解析得到的tsv文件的文件头部（位于tsv文件的第一行，指示了后序每行数据每列代表的内容）

![pcap解析得到的tsv文件的文件头](https://gitee.com/songz7026/image-pool/raw/master/Python/xsv_tsv_header.jpg)

## 二、Python I/O 简介（文件读取）

涉及到对xsv文件的操作，需要了解Python文件读取的主要方法。

### 1、open(filename, mode, ...)
```
open(filename, mode, ...)
	(<PATH>, "rwxts...", ...)//mode可以参看builtins.py源代码中的注释
```

+ `return`: 方法返回一个`file`对象，该对象可以进行进一步操作。
+ `filename`: 操作的的文件路径
+ `mode`: 文件操作模式选项，可以进一步细分为`rwx...`
```
========= ===============================================================
Character Meaning 来自builtins.py中open()方法下的注释
--------- ---------------------------------------------------------------
文件打开模式，特别注意光标的位置（光标向后的位置会被覆盖）
'r'       open for reading (default) 只读(默认) 光标位于开头
'w'       open for writing, truncating the file first 只写 光标位于开头 原有内容删除 若文件不存在则新建文件
'x'       create a new file and open it for writing 只写 新建文件 如果该文件已存在则会报错
'a'       open for writing, appending to the end of the file if it exists 只写（追加append） 光标位于最后 若文件不存在则新建文件

返回文件的形式
'b'       binary mode 返回binary文件， 对于'byte'组成的文件
't'       text mode (default) 返回文本文件， 对于"str"组成的文件 
覆盖
'+'       open a disk file for updating (reading and writing) 打开一个文件进行更新(可读可写)

其它
'U'       universal newline mode (deprecated) 通用换行模式
========= ===============================================================
```
关于open()方法更详细的操作可以参考
1. 源代码( *在编辑器中查找open()的定义位置* )中的注释，
2. [官方文档](https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files)
3. [菜鸟教程：Python open() 函数](https://www.runoob.com/python/python-func-open.html)

#### `file`对象方法

+ `file.read([size])`：size 未指定则返回整个文件，如果文件大小 >2 倍内存则有问题，f.read()读到文件尾时返回""(空字串)。
+ `file.readline()`：返回一行。
+ `file.readlines([size])`：返回包含size行的列表, size 未指定则返回全部行。
+ `for line in f: print line` ：通过迭代器访问。
+ `f.write("hello\n")`：如果要写入字符串以外的数据,先将他转换为字符串。
+ `f.tell()`：返回一个整数,表示当前文件指针的位置(就是到文件头的字节数)。
+ `f.seek(偏移量,[起始位置])`：用来移动文件指针。
偏移量: 单位为字节，可正可负
起始位置: **0** - 文件头, 默认值; **1** - 当前位置; **2** - 文件尾
+ `f.close()`： 关闭文件

### 2、with关键字
>引子：使用内置open()打开文件之后，要close()以正确关闭以释放资源

`open()`操作常常与`with`关键字连用，解决引子中麻烦的累赘问题

+ 对比之后，显而易见

```
try open():
	...
finally:
	close()
```

```
with open()
	...//更加简洁，自动关闭
```
## 三、Python对xsv的支持

在Python中，有对csv的支持，其它变体（包括tsv）一律视为其中的`delimiter`等参数不同。
```
import csv
```
[Python csv Document](https://docs.python.org/3/library/csv.html)

[Python csv 官方文档](https://docs.python.org/zh-cn/3/library/csv.html)

### 1、csv文件的读取

```
csv.reader(csvfile, dialect='excel', **fmtparams)
```

+ `return`: `reader`对象
+ `csvfile`: 文件路径
+ `dialect`: 指明读取的对象是何种csv文件的变体，如tsv。
+ `**fmtparams`：**变种与格式参数** 覆盖当前变种(dialect)的单个格式设置

>Python认定csv格式的各种变种都是通过改了一堆参数得到的，（参见python文档），如
>>dialect.delimiler 用于分隔字段的单字符，默认为逗号","
>>dialect.doublequole ...
>>dialect.escapechar ...
>>..

### 2、iterator迭代器
引子：csv.reader()使用的对象只需要下列两点，即可顺利打开
> 1. 满足iterator协议
> 2. 并且每次都调用`__next__()`方法都返回`str` 
> 
> 关键字：iterator协议；\_\_next()\_\_

**迭代器对象**：表示一串数据的对象，如`csv.reader()`返回的`reader`对象。配合使用`__next__()`方法即可逐行获得字符串。

![iterator对象示意图](https://gitee.com/songz7026/image-pool/raw/master/Python/xsv_iterator.png)
+ `__next__()`：返回迭代器对象中的下一项，到达最后一项溢出会引发`StopIteration`异常

### 3、csv.reader()和csv.writerow()

```
>> datasets1 = '...' # 设置文件路径
>> obj_file1 = open(datasets1) # 获得file类型文件obj_file
>> obj_reader = csv.reader(obj_file1) # 获得reader类型文件；reader(csvfile)接受csvfile类型作为参数，file类型被向下转换到csvfile类型

>> datasets2 = '...' # 设置文件路径
>> obj_file2 = open(datasets2,'w') 
>> obj_file2.writerow([1,2,33])
>> obj_file2.writerow([1,3,5])
>> obj_file2.writerow([6,4,2])
```

经过测试，发现再writerow()后还插入了一个空行，导致插入多行时会产生大量冗余的空行。

```
# 生成的文件
1，2，33

1，3，5

6，4，2
```
查看文档发现是由于没有在`open()`阶段设置换行符参数`newline`，因为不同的系统有不同的换行符，如`Windows`采用`CRLF`(回车换行'\r\n')，`Linux`采用`LF`(换行'\n')。

```
>> datasets = '...'
>> obj_file = open(dataset,'w',newline='')
>> obj_file2.writerow([1,2,33])
>> obj_file2.writerow([1,3,5])
>> obj_file2.writerow([6,4,2])
```

```
# 生成的文件
1，2，33
1，3，5
6，4，2
```

### 4、遍历csv

笔者利用迭代器的__next__()方法在最后一行之后继续调用会产生`StopIteration`异常这一特性，想出了如下代码以遍历整个csv文件。

```

try:
	while True:
		datasets = '...' # 设置文件路径
		obj_file = open(datasets) # 获得file类型文件obj_file
		obj_reader = csv.reader(obj_file) # 获得reader类型文件；reader(csvfile)接受csvfile类型作为参数，file类型被向下转换到csvfile类型

		row = obj_reader.__next__()
except StopIteration:
	pass
```

### 5、其它

笔者在测试过程中，发现单纯使用`open()`得到的`file`类型也有迭代器方法`__next__()`，因此将二者做了对比如下。

```
>> datasets = '...' # 设置文件路径
>> obj_file = open(datasets) # 获得file类型文件obj_file
>> obj_reader = csv.reader(obj_file) # 获得reader类型文件；reader(csvfile)接受csvfile类型作为参数，file类型被向下转换到csvfile类型

>> obj_file.__next__()
'0,tcp,http,SF,181,5450,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,0,8,8,0.00,0.00,0.00,0.00,1.00,0.00,0.00,9,9,1.00,0.00,0.11,0.00,0.00,0.00,0.00,0.00,normal.\n'

>> obj_reader.__next__()
['0', 'tcp', 'http', 'SF', '239', '486', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '8', '8', '0.00', '0.00', '0.00', '0.00', '1.00', '0.00', '0.00', '19', '19', '1.00', '0.00', '0.05', '0.00', '0.00', '0.00', '0.00', '0.00', 'normal.']

>> obj_file.__next__()
'0,tcp,http,SF,235,1337,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,0,8,8,0.00,0.00,0.00,0.00,1.00,0.00,0.00,29,29,1.00,0.00,0.03,0.00,0.00,0.00,0.00,0.00,normal.\n'

>> obj_reader.__next__()
['0', 'tcp', 'http', 'SF', '219', '1337', '0', '0', '0', '0', '0', '1', '0', '0', '0', '0', '0', '0', '0', '0', '0', '0', '6', '6', '0.00', '0.00', '0.00', '0.00', '1.00', '0.00', '0.00', '39', '39', '1.00', '0.00', '0.03', '0.00', '0.00', '0.00', '0.00', '0.00', 'normal.']

>> type(obj_file.__next__())
<class 'str'>

>> type(obj_reader.__next__())
<class 'list'>

```

```
# 实验使用的KDD99_10_percent 最初的几行
0,tcp,http,SF,181,5450,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,0,8,8,0.00,0.00,0.00,0.00,1.00,0.00,0.00,9,9,1.00,0.00,0.11,0.00,0.00,0.00,0.00,0.00,normal.
0,tcp,http,SF,239,486,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,0,8,8,0.00,0.00,0.00,0.00,1.00,0.00,0.00,19,19,1.00,0.00,0.05,0.00,0.00,0.00,0.00,0.00,normal.
0,tcp,http,SF,235,1337,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,0,8,8,0.00,0.00,0.00,0.00,1.00,0.00,0.00,29,29,1.00,0.00,0.03,0.00,0.00,0.00,0.00,0.00,normal.
0,tcp,http,SF,219,1337,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,0,6,6,0.00,0.00,0.00,0.00,1.00,0.00,0.00,39,39,1.00,0.00,0.03,0.00,0.00,0.00,0.00,0.00,normal.
0,tcp,http,SF,217,2032,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,0,6,6,0.00,0.00,0.00,0.00,1.00,0.00,0.00,49,49,1.00,0.00,0.02,0.00,0.00,0.00,0.00,0.00,normal.
0,tcp,http,SF,217,2032,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,0,6,6,0.00,0.00,0.00,0.00,1.00,0.00,0.00,59,59,1.00,0.00,0.02,0.00,0.00,0.00,0.00,0.00,normal.
0,tcp,http,SF,212,1940,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,0,1,2,0.00,0.00,0.00,0.00,1.00,0.00,1.00,1,69,1.00,0.00,1.00,0.04,0.00,0.00,0.00,0.00,normal.
0,tcp,http,SF,159,4087,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,0,5,5,0.00,0.00,0.00,0.00,1.00,0.00,0.00,11,79,1.00,0.00,0.09,0.04,0.00,0.00,0.00,0.00,normal.
0,tcp,http,SF,210,151,0,0,0,0,0,1,0,0,0,0,0,0,0,0,0,0,8,8,0.00,0.00,0.00,0.00,1.00,0.00,0.00,8,89,1.00,0.00,0.12,0.04,0.00,0.00,0.00,0.00,normal.
0,tcp,http,SF,212,786,0,0,0,1,0,1,0,0,0,0,0,0,0,0,0,0,8,8,0.00,0.00,0.00,0.00,1.00,0.00,0.00,8,99,1.00,0.00,0.12,0.05,0.00,0.00,0.00,0.00,normal.
...
```

通过对比可以发现，数据集获得的`file`对象和`reader`对象关于迭代方法的异同有

1. 共同点：均可迭代
2. 共同点：实验中两个有关联的对象的迭代指针是**共享**的。

3. 不同点：返回的类型不同。分别是`str`和`list`

进一步了解这涉及到了Python中重要的**迭代器**和**生成器**，Python中很多数据类型都可以作为迭代器，关于更详细的迭代器的内容与本文不大，不做赘述。

## 四、参考资料
[1]. [wikipida: 制表符分隔值](https://zh.wikipedia.org/wiki/%E5%88%B6%E8%A1%A8%E7%AC%A6%E5%88%86%E9%9A%94%E5%80%BC)
[2]. [wikipida: 逗号分隔值](https://zh.wikipedia.org/wiki/%E9%80%97%E5%8F%B7%E5%88%86%E9%9A%94%E5%80%BC)
[3]. [菜鸟教程：Python open() 函数](https://www.runoob.com/python/python-func-open.html)