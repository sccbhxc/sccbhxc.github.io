---
layout: post
title: Caffe常用Python接口总结
category: 框架
tag: Deep-Learning
description: 总结了Caffe中常用的接口
---

## 概述

由于Caffe官网上没有详细的Python接口函数文档，为了方便Caffe接口的查阅，将Caffe的常用接口进行了整理。通常在python环境下，可以通过help(object)函数，来可以查询对应的库函数的说明文档。例如，调用help(caffe.Net)，就可以查询到caffe.Net的说明文档。



## Caffe.Net

### Member Function

```python
__init__(...)

# Callbacks，回调
after_backward()
after_forward()
before_backward()
before_forward()

# 网络运行
backward() # prepare inputs and run the net forward
forward() # prepare diffs and run the net backward.

forward_all() # Run net forward in batches.
forward_backward_all() # Run net forward + backward in batches.

# 对学习参数进行处理
clear_param_diffs(...)
copy_from(...)

# 保存参数
load_hdf5(...)
save_hdf5(...)
save(...)

# 分配内存
reshape(...)

set_input_arrays() # Set input arrays of the in-memory MemoryDataLayer
share_with(...)
```



### Member Variable

```python
blob_loss_weights # OrderedDict, blob loss weights indexed by name
blobs # OrderedDict, blobs indexed by name,保存了feature map和gradient

layer_dict # OrderedDict, layers indexed by name
layers # 保存了网络参数，例如卷积核，偏置
params # OrderedDict, parameters indexed by name; each is a list of multiple blobs (e.g.,weights and biases)，保存了网络参数，例如卷积核，偏置

inputs
outputs

bottom_names # str类型的list
top_names
```



## Caffe.Classifier ( Caffe.Net的子类 )

Classifier是Net的子类，包含所有caffe.Net的可访问成员函数和成员函数，相比较于父类Net，Classifier增加了数据预处理，并多了predict(...)这个封装接口。

### Member Function

```python
# 包含所有caffe.Net的可访问成员函数
predict(...) 
```

### Member Variable

```python
# 包含所有caffe.Net的可访问成员变量
```



## Caffe.SGDSolver

### Member Function

```python
__init__(...)

# Invoked at specific points during an iteration
add_callback(...)

restore(...) # restore the state from the appropriate snapshot type.
snapshot(...) # produces a SolverState protocol buffer that needs to be written to disk together with the learned net.

solve(...) # resume training for a pre-trained net
step(...)
```



### Member Variable

```python
iter #
net # 
param #
test_nets #
```



### 其他类似的类

- Caffe.NesterovSolver
- Caffe.AdaGradSolver
- Caffe.RMSPropSolver
- Caffe.AdaDeltaSolver




## Caffe.io.Transformer

### Member Function

```python
__init__(self, inputs)
deprocess(self, in_, data) # Invert Caffe formatting
preprocess(self, in_, data) # Format input for Caffe
set_channel_swap(self, in_, order) # Set the input channel order for e.g. RGB to BGR conversion
set_input_scale(self, in_, scale) # Set the scale of preprocessed inputs s.t. the blob = blob * scale.
set_mean(self, in_, mean) # Set the mean to subtract for centering the data.
set_raw_scale(self, in_, scale) # Set the scale of raw features s.t. the input blob = input * scale.
set_transpose(self, in_, order) # Set the input channel order for e.g. RGB to BGR conversion
```



## Caffe.io API

```python

array_to_datum(arr, label=None) # Converts a 3-dimensional array to datum.
datum_to_array(datum) # Converts a datum to an array.

array_to_blobproto(arr, diff=None) # Converts a N-dimensional array to blob proto.
blobproto_to_array(blob, return_diff=False) # Convert a blob proto to an array.

arraylist_to_blobprotovector_str(arraylist) # Converts a list of arrays to a serialized blobprotovec, which could be then passed to a network for processing.
blobprotovector_str_to_arraylist(str) # Converts a serialized blobprotovec to a list of arrays.

load_image(filename, color=True) # Load an image converting from grayscale or alpha as needed.
oversample(images, crop_dims) # Crop images into the four corners, center, and their mirrored versions.
resize_image(im, new_dims, interp_order=1) # Resize an image array with interpolation.
```



## Other

### Caffe.Timer

```python
# Member Function
start(...) # 开始计时
stop(...) # 停止计时

# Member Variable
ms # start()到stop()的时间，单位ms
```

### Caffe.Layer

caffe.Layer往往作为父类，用于定义用户自己的python层。自定义的层，需要重写setup(...), forward(...), reshape(...)和backward(...)这4个接口。

```python
# Member Function
__init__(...)
reshape(...)# 分配内存
setup(...)# 初始化layer参数

# Member Variable
blobs # 保存feature map
type # layer的层类型
```

### 用于定义网络结构，生成prototxt

```python
# generates functions that specify layers; 
# e.g., Layers().Convolution(bottom, kernel_size=3) 
# will produce a Top specifying a 3x3 convolution applied to bottom.
caffe.layers

caffe.params # 用于声明layers的参数

caffe.NetSpec # 包含to_proto()成员函数，用于将网络参数输出为prototxt

# Generate a NetParameter that contains all 
# layers needed to compute all arguments.
to_proto()
```



## API

```python
# 日志
init_log()
log()

# 设置计算设备
set_mode_cpu()
set_mode_gpu()
set_device(...)
set_multiprocess(...) # 设置使用几个cpu核心

# solver,Parallel training
get_solver(...)
solver_count()
set_solver_count()
solver_rank()
set_solver_rank()

# 其他
layer_type_list()
set_random_seed() # Sets the random seed of both boost and curand
```



## Examples

**使用这些API的示例代码见后续博客**

