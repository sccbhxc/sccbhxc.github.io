---
layout: post
title: Tensorflow数据读取
category: 框架
tag: Deep-Learning
description: 总结Tensorflow数据批量读取的方法
---

## 数据读取

Tensorflow数据读取的常用方式有以下2种：

1. **预加载数据**：将需要使用的数据一次性加载到内存中；
2. **队列读取**：设置Tensorflow数据读取queue，tensorflow在训练的时候，作为Graph的一部分，建立数据读取流水线。

**比较**：方法1在数据量比较少、内存足够的情况下比较适用。而方法2具有更大的灵活性，可以同时适应大数据集和小数据集。同时，方法2不会因为集中读取数据集而产生较长的时间延迟。

使用**方法2**读取数据，首先需要将图像数据保存成tensorflow使用的TFRecords。TFRecords格式是一种二进制文件，对内存的利用更加高效，方便tensorflow读取数据和label。TFRecords文件中包含了tf.train.Example协议内存块(protocol buffer，其中包含了tf.train.Features字段)。其建立过程如下：

1. 将数据和label封装成tf.train.Features；
2. 将1中封装得到的tf.train.Features封装成tf.train.Example；
3. 将tf.train.Example协议内存块序列化为字符串，最后写入到TFRecords文件中。

以下是<u>将数据保存为TFRecords文件</u>的具体实现代码：

```python
def create_record_train():
    '''
    将train.txt文件转换成train.tfrecords
    '''
     record_path='data/train.txt'
    file = open(record_path, 'r')
    file_strs = file.read().split('\r\n')
    file_len = len(file_strs)-1

	save_name='data/train.tfrecords'
    writer = tf.python_io.TFRecordWriter(save_name)
    for s in file_strs[0:-1]:
        img_name = 'data/train/' + s[0:-2]
        label = int(s[-1])
        img = Image.open(img_name)
        img_raw = img.tobytes() #将图片转化为原生bytes
        example = tf.train.Example(features=tf.train.Features(feature={
            "label": tf.train.Feature(int64_list=tf.train.Int64List(value=[label])),
            'img_raw': tf.train.Feature(bytes_list=tf.train.BytesList(value=[img_raw]))
        }))
        writer.write(example.SerializeToString())
    writer.close()
```



以下是<u>读取TFRecords文件</u>的具体实现代码：

```python
def read_and_decode(filename):
    filename_queue = tf.train.string_input_producer([filename])

    reader = tf.TFRecordReader()
    _, serialized_example = reader.read(filename_queue)
    features = tf.parse_single_example(serialized_example,
                                       features={
                                           'label': tf.FixedLenFeature([], tf.int64),
                                           'img_raw' : tf.FixedLenFeature([], tf.string),
                                       })

    img = tf.decode_raw(features['img_raw'], tf.uint8)
    img = tf.reshape(img, [227, 227, 1])
    img = tf.cast(img, tf.float32) * (1. / 255) - 0.5
    label = tf.cast(features['label'], tf.int32)

    return img, label
```



调用方式

```python
img, label = read_and_decode("data/train.tfrecords")

img_batch, label_batch = tf.train.shuffle_batch([img, label],
                                                batch_size=10, capacity=2000,
                                                min_after_dequeue=1000)
#初始化所有的op
init = tf.initialize_all_variables()
with tf.Session() as sess:
    sess.run(init)
    #启动队列
    coord = tf.train.Coordinator()
    threads = tf.train.start_queue_runners(sess=sess, coord=coord)
    for i in range(3):
        val, l= sess.run([img_batch, label_batch])
        #l = to_categorical(l, 12)
        print(val.shape, l)
    coord.request_stop()
    coord.join(threads)
```

**如果只读取单张图片，有一种更加简便的方法**

```python
def load_batch_image(batch_size, height, width, path, preprocess_fn, epochs=2, shuffle=True):
    filenames = [join(path, f) for f in listdir(path) if isfile(join(path, f))]
    if not shuffle:
        filenames = sorted(filenames)

    # png = filenames[0].lower().endswith('png')  # If first file is a png, assume they all are
    jpg = filenames[0].lower().endswith('jpg')  

    filename_queue = tf.train.string_input_producer(filenames, shuffle=shuffle, num_epochs=epochs)
    reader = tf.WholeFileReader()
    _, img_bytes = reader.read(filename_queue)
    # image = tf.image.decode_png(img_bytes, channels=3) if png else tf.image.decode_jpeg(img_bytes, channels=3)
    image = tf.image.decode_jpeg(img_bytes, channels=3)

    processed_image = preprocess_fn(image, height, width)
    return tf.train.batch([processed_image], batch_size, dynamic_pad=True)
```


## 后续更新。。。