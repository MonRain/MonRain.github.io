---
title: keras.layers的核心网络层
categories: tensorflow
tags: 
- tensorflow
- keras
- layout
---

# 网络层

## Dense层

最普通最常见的**全连接层**。

**输入尺寸**：nD 张量，尺寸: `(batch_size, ..., input_dim)`。 最常见的情况是一个尺寸为 `(batch_size, input_dim)` 的 2D 输入。

**输出尺寸**：nD 张量，尺寸: `(batch_size, ..., units)`。 例如，对于尺寸为 `(batch_size, input_dim)` 的 2D 输入， 输出的尺寸为 `(batch_size, units)`。

```
keras.layers.Dense(units, activation=None, use_bias=True, kernel_initializer='glorot_uniform', bias_initializer='zeros', kernel_regularizer=None, bias_regularizer=None, activity_regularizer=None, kernel_constraint=None, bias_constraint=None)
```

+ `units`: **正整数**。输出空间维度。
+ `activation`: **字符串**。[激活函数](https://keras.io/zh/activations/)。默认不使用激活函数，即线性激活`act(x)=x`。

可用参数有 `
'softmax', 'elu', 'selu', 'softplus', 'softsign', 'relu', 'tanh', 'sigmoid', 'hard_sigmoid', 'exponential', 'linear'
`

+ `use_bias`: **布尔值**。是否使用偏置变量。
+ `kernel_initializer`: `kernel`*权重矩阵*[**初始化器**](https://keras.io/zh/initializers/)。
+ `bias_initializer`: *偏置向量*的**初始化器**
+ `kernel_regularizer`: 运用到 `kernel` *权值矩阵*的[**正则化函数**](https://keras.io/zh/regularizers/)
+ `bias_regularizer`: 运用到*偏置向量*的**正则化函数**
+ `activity_regularizer`: 运用到层的*输出*的**正则化函数** 
+ `kernel_constraint`: 运用到 `kernel` *权值矩阵*的[**约束函数**](https://keras.io/zh/constraints/)
+ `bias_constraint`: 运用到*偏置向量*的**约束函数**

## Activation层

激活层，相当于一层激活函数。

**输入尺寸**：任意尺寸。 当使用此层作为模型中的第一层时， 使用参数 `input_shape` （整数元组，不包括样本数的轴）。

**输出尺寸**：与输入相同。

```
keras.layers.Activation(activation)
```

+ 参数与其它层的**激活函数**参数相同。

## Dropout层

Dropout 应用于输入。在训练中每次更新时，将输入单元的**按比率随机设置为 0**，这有助于防止**过拟合**。

```
keras.layers.Dropout(rate, noise_shape=None, seed=None)
```

+ **rate**: **[0 ~ 1]**。需要丢弃的输入比例。
+ [**noise_shape**](https://blog.csdn.net/weixin_42041786/article/details/103516660): 1D 整数张量，代表了随机产生“保留/丢弃”标志的shape。
+ **seed**: 一个作为随机种子的 Python 整数。


## Flatten层

将输入展平。不影响批量`batch_size`大小。

```
keras.layers.Flatten(data_format=None)
```

+ **data_format**: **字符串**。

## 其它
```
keras.engine.input_layer.Input()

keras.layers.Reshape(target_shape)

keras.layers.Permute(dims)

keras.layers.RepeatVector(n)

keras.layers.Lambda(function, output_shape=None, mask=None, arguments=None)

keras.layers.ActivityRegularization(l1=0.0, l2=0.0)

keras.layers.Masking(mask_value=0.0)

keras.layers.SpatialDropout1D(rate)

keras.layers.SpatialDropout2D(rate)

keras.layers.SpatialDropout3D(rate)
```

# 参考资料

[1]. [Docs » Layers » 核心网络层](https://keras.io/zh/layers/core/)
[2]. [dropout的noise_shape含义](https://blog.csdn.net/weixin_42041786/article/details/103516660)
[3]. [tf.nn.dropout()的用法](https://blog.csdn.net/yangfengling1023/article/details/82911306)