---
layout: post
title: Caffe常用Python接口用法总结
category: 框架
tag: Deep-Learning
description: 总结了Caffe中常用接口的用法
---

## 概述

根据Caffe的日常使用情况，对常用的python接口进行了总结整理，以方便后续科研工作中查阅。



## Caffe初始化设置

```python
# ------------- 设置计算设备 ------------- #
caffe.set_mode_gpu()               # 使用GPU，注意默认使用CPU，要使用GPU就不能缺少这一句
caffe.set_mode_cpu()               # 使用CPU
caffe.set_device(device_id)        # 不设置默认为0

```



## Model训练

```python
# --------------- Solver --------------- #

# ------- 加载Solver，有2种方法 -------
# 1. 无论Solver类型是什么，统一设置为SGD
solver = caffe.SGDSolver('path/to/solver.prototxt')
# 2. 根据solver.prototxt中指定的Solver类型读取，默认为SGD
solver = caffe.get_solver('path/to/solver.prototxt')

# ------- 前向传播 -------
solver.net.forward()             # train net forward
solver.test_nets[0].forward()    # test net forward, test net允许有多个(train net只能有1个)

# ------- 反向传播 -------
solver.net.backward()

# ------- 模型训练 -------
solver.step(n)                   # 模型进行n次forward和backward，完成n次训练
solver.solver()                  # 模型根据solver.prototxt中的设置，进行完整模型训练

# ------- 模型保存 -------
solver.net.save('name.caffemodel')
```





## 加载已训练Model

```python
# ------- 从已有caffemodel中加载 -------
net = caffe.Net(
		deploy_prototxt_path,  # deploy网络定义prototxt文件
		caffe_model_path,      # 已训练模型的caffemodel文件
		caffe.TEST             # phase设置为Test
		)
# ------- 从已有solverstate中恢复训练 -------
solver.restore('path/to/solver.solverstate')
net2.copy_from('path/to/net.caffemodel')
# ------- 从已有Net对象中共享得到 -------
net2.share_with(net1)           # net2共享net1的权重(权重指针指向同一地址)
```



## 数据预处理

```python
# ------- 读取均值文件 -------
mean_blob = caffe.proto.caffe_pb2.BlobProto()
mean_blob.ParseFromString(open('mean.binaryproto','rb').read())
mean_npy = caffe.io.blobproto_to_array(mean_blob)

# ------- 图片预处理 -------
from scipy.misc import imread, imresize
img = imread(filename)
img = imresize(img, dst_size)

if len(img.shape) == 3:
  mean_val = np.mean(mean_npy, axis=(2,3))
  img -= mean_val[:,np.newaxis,np.newaxis]*np.ones(img.shape) # ndarray broadcasting
  img_batch = np.array([img.transpose((2,0,1))])
  img_batch.astype(np.float32)
elif len(img.shape) == 2:
  mean_val = np.mean(mean_npy)
  img -= mean_val
  img_batch = np.array([img[np.newaxis,:,:]])
  img_batch.astype(np.float32)
  
net.blobs['data'].data[...] = img_batch
```



## 访问网络

```python
# ------- 网络参数 -------
conv1_weight = net.params['conv1'][0].data # type: np.ndarray
conv1_bias = net.params['conv1'][1].data

# ------- 网络参数的梯度 -------
conv1_weight = net.params['conv1'][0].diff # type: np.ndarray
conv1_bias = net.params['conv1'][1].diff

# ------- feature map -------
conv1_feat = net.blobs['conv1'].data[0] # type: np.ndarray
conv1_feat = net.blobs['conv1'].diff[0] # 梯度

for layer_name, param in net.params.items():
  print layer_name + '\t' + str(param[0].data.shape), str(param[1].data.shape)

for layer_name, blob in net.blobs.items():
  print layer_name + '\t' + str(blob.data.shape)
```





## Reference

1. [Caffe-Python接口常用API参考](http://wentaoma.com/2016/08/10/caffe-python-common-api-reference/)

