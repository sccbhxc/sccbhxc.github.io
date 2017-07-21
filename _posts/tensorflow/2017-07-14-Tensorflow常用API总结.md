---
layout: post
title: Tensorflow常用API总结
category: 框架
tag：Deep-Learning
description: 总结Tensorflow中常用API
---


## 常用 Tensor 操作

可以将tensorflow中的tensor操作和numpy中的矩阵操作对应

```python
tf.size()  # This operation returns an integer representing the number of elements in input.
tf.shape()  # Returns the shape of a tensor.

tf.reshape()  # Reshapes a tensor.
tf.squeeze()  # Removes dimensions of size 1 from the shape of a tensor.
tf.expand_dims()  # Inserts a dimension of 1 into a tensor's shape.

tf.concat()  # Concatenates tensors along one dimension.
tf.split()  # Splits a tensor into sub tensors.
tf.tile()  # Constructs a tensor by tiling a given tensor.

tf.slice()  # Extracts a slice from a tensor.
tf.stack()  # Stacks a list of rank-R tensors into one rank-(R+1) tensor. v1.0之前为tf.pack()
tf.gather()  # Gather slices from params according to indices.
tf.pad()  # Pads a tensor.


tf.reduce_sum()  # Computes the sum of elements across dimensions of a tensor.
tf.reduce_mean() # Computes the mean of elements across dimensions of a tensor.
tf.reduce_max()  # Computes the maximum of elements across dimensions of a tensor.

tf.argmax()  # Returns the index with the largest value across axes of a tensor.
tf.argmin()  # Returns the index with the smallest value across axes of a tensor.

tf.fill()  # Creates a tensor filled with a scalar value.

tf.random_shuffle()  # Randomly shuffles a tensor along its first dimension.
tf.truncated_normal()
tf.random_normal()
```



## 其他

```python
tf.cast()  # Casts a tensor to a new type.
tf.to_float()  # Casts a tensor to type float32.
tf.equal()  # Returns the truth value of (x == y) element-wise.
tf.clip_by_value() # Clips tensor values to a specified min and max.
tf.assign()  # Update 'ref' by assigning 'value' to it.
```



## 参考

1. [Tensorflow官方教程：Reading data](https://www.tensorflow.org/programmers_guide/reading_data)
2. https://github.com/tensorflow/tensorflow/blob/r1.2/tensorflow/examples/how_tos/reading_data/fully_connected_reader.py
3. https://github.com/tensorflow/tensorflow/blob/r1.2/tensorflow/examples/how_tos/reading_data/convert_to_records.py

