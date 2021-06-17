---
title: '厘清Keras中的shape,unit,input_shape'
date: 2021-03-05 16:06:22
categories: TensorFlow
tags:
- Keras
- TensorFlow
---

# 厘清Keras中的shape,unit,input_shape

新人（没错正是在下）在个性化Github上的Keras代码时，常常会遇到各种输入输出维度的报错，因此打算仔细梳理一下关于里面的shape, unit(Dense等Layer层的必需参数), input_shape, input_dim等参数。

## 顾名思义TensorFlow

由于TensorFLow以及将Keras封为御用前端，因此笔者的所有Keras代码都是基于`tensorflow.keras`包实现的。但这里要说的不是二者的关系，而是借助这个`tensorflow`这个名称了解Keras的各层之间的运行关系。

Tensor本意为“张量”，可以理解为高维度的向量，也可姑且理解为一组带着奇怪复杂形状的数据。而Flow本意为“流”，二者组成TensorFlow顾名思义解释为**张量流**。

也就是说，我们可以吧机器学习模型的运行看成一些**张量**组成的**流**。

这样讲还是很抽象，不过经过下面对各种shape的解释之后就很容易理解了。

## shape

shape在keras中是一个`turple`。

`shape=(30,4,10) means` 意味着一个三维数组：

+ 维度1 - 30长度
+ 维度2 - 4长度
+ 维度3 - 10长度

对于特定的层来说，可以去参考文档。这里列举几个常用的玩意儿。

> + Dense layers: (batch_size, input_size)
or (batch_size, optional,...,optional, input_size)
> + 2D convolutional layers:
    if using channels_last: (batch_size, imageside1, imageside2, channels)
    if using channels_first: (batch_size, channels, imageside1, imageside2)
> + 1D convolutions and recurrent layers: (batch_size, sequence_length, features)

[Details on how to prepare data for recurrent layers](https://stackoverflow.com/a/50235563/2097240)

一般来说就是`(batch_size, bulabula)`

## 问题：第一层的Dense(unit,...)和input_shape有啥区别呢？

这就体现出最初的顾名思义TensorFlow了。

![本图片非原创，来源于参考链接](https://gitee.com/songz7026/image-pool/raw/master/Python/Keras_unit_inputshape.jpg)

对于

```
Dense(unit,input_shape,...)
```

`unit`: 就是该层的单元数，也即该层输出的尺寸。
`input_shape`: 其实这个参数和这个层**没什么关系**，它的使命是告诉这个层未来的输入应该是什么样的（这句话像是废话）。下面详细说一说这个`input_shape`：

在Keras中，我们定义的都是`Layers`，也就是各个隐藏层。但是`Layers`之间的各种复杂关系我们不需要关心，但是在这里我们说一个概念：层之间流动了是**Tensor**，也就是一堆**上一层计算得到**的复杂形状的数据流给**下一层**供接下来计算。

**我们定义的第一个Layers其实并不是输入层(Input Layer), 而是第一个隐藏层(hidden layer 1)。**

因此在此之前的输入层到第一隐藏层的**Tensor**是Keras接触不到的，只能由智慧的你来告诉他，因此才出现了`Input`层，实际上它是一个**Tensor**。

## 参考

[Stack overflow: Keras input explanation: input_shape, units, batch_size, dim, etc](https://stackoverflow.com/questions/44747343/keras-input-explanation-input-shape-units-batch-size-dim-etc)