---
layout:     post
title:      "简单粗暴Tensorflow2.0学习笔记"
date:       2019-11-15 10:00:00
author:     "Xindi"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Tensorflow
---


#### 资源链接
[github地址](https://github.com/snowkylin/tensorflow-handbook)
[Tensorflow官方文档](https://www.tensorflow.org/guide/eager)
#### Tensorflow安装

```
# 创建一个tensorflow的环境
conda create -n tensorflow python=*.*

# cpu
pip install tensorflow

# gpu
pip install tensorflow-gpu

# tensorflow升级
pip install --upgrade tensorflow

# 查看tensorflow version
python
import tensorflow as tf
print (tf.__version__)
```

#### eager execution
在tensorflow 2.0中, eager execution是默认开启的
``` python
import tensorflow as tf
tf.execting_eagerly()
true
```

第一个程序
```python
import tensorflow as tf
x = [[2.]]
m = tf.matmul(x, x)
print("hello, {}".format(m))
```

可以run Tensorflow ops并且立即返回结果.
Eager execution能够很好与Numpy兼容: Numpy的操作接受tf.Tensor, Tensorflow的数学运算将Python对象以及Numpy的数组转为tf.Tensor对象.
tf.Tensor.numpy方法能够将对象的值作为一个Numpy数组返回

```python
# Obtain numpy value from a tensor:
print(a.numpy())
# => [[1 2]
#     [3 4]]
```

##### eager training
使用tf.GradientTape()去即刻计算梯度.

```python
w = tf.Variable([[1.0]])
with tf.GradientTape() as tape:
  loss = w * w

grad = tape.gradient(loss, w)
print(grad)  # => tf.Tensor([[ 2.]], shape=(1, 1), dtype=float32)
```



论文地址: https://arxiv.org/pdf/1805.10727.pdf
