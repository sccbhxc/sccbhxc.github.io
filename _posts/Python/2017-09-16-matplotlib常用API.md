---
layout: post
title: matplotlib常用API
category: 编程
tags: Programming
desciption: 
---

### 1. 类Matlab API
```python
import matplotlib.pyplot as plt
from pylab import *

plot(x, y, 'r*-')

xlabel('x')
ylabel('y')
title('title')

subplot(1,2,1)
# subplot(121)

plt.show() # 注意与plt.imshow()的区别（用于显示图像或者二维数据）
```



### 2. matplotlib面向对象 (指定局部绘图对象)



```python
fig = plt.figure()

axes = fig.add_axes([0.1, 0.1, 0.8, 0.8]) # left, bottom, width, height (range 0 to 1) 
# axes = fig.add_axes()

axes.plot(x, y, 'r')

axes.set_xlabel('x') # 设置轴标
axes.set_ylabel('y')
axes.set_title('title') # 设置标题
```



### 3. 同时放置多个图表



```python
fig, axes = plt.subplot(nrow=1, ncols =2)

for ax in axes:
  ax.plot(x, y, 'r')
  ax.set_xlabel('x')
  ax.set_ylabel('y')
  ax.set_title('title')

fig.tight_layout() # 紧凑布局
```



### 4. 设置图标的尺寸

```python
fig = plt.figure(figsize=(8,4), dpi=100)
```



### 5. 保存图表

```python
plt.savefig('filename.png') # 支持格式PNG，JPG，EPS，SVG，PGF 和 PDF
plt.sacefig('filename.png', dpi=200)
```



### 6. 设置图例

```python
# 简单方法
ax.legend(['curve1', 'curve2', 'curve3'])

# 准确指定方法
ax.plot(x1, y1, label='curve1')
ax.plot(x2, y2, label='curve2')
ax.legend()

# 制定图例的位置
ax.legend(loc=0) # let matplotlib decide the optimal location
ax.legend(loc=1) # upper right corner
ax.legend(loc=2) # upper left corner
ax.legend(loc=3) # lower left corner
ax.legend(loc=4) # lower right corner
```



### 7. 字体设置

```python
from matplotlib import rcParams
# 修改全局字体
rcParams.update({'font.size': 18, 'font.family': 'serif'})
rcParams.update({'font.size': 18, 'font.family': 'STIXGeneral', 'mathtext.fontset': 'stix'})

# 用Latex渲染
rcParams.update({'font.size': 18, 'text.usetex': True})

```



### 8. 图像显示

```python
plt.imshow(img, cmap='gist_earth', interpolation='nearest')
# cmap: gist_heat, gist_gray, gray, hot, jet
```

 



### 参考

1.  [matplotlib绘图总结](http://whuhan2013.github.io/blog/2016/09/16/python­-matplotlib­-learn/)