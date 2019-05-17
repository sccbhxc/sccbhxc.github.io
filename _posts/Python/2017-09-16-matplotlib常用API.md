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
fig = plt.figure(figsize=(8,4), dpi=100) # 输出的figure窗口的大小
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

# 设置自定义字体
import matplotlib
myfont = matplotlib.font_manager.FontProperties(fname=r'C:/Windows/Fonts/msyh.ttf') 
```



### 8. 图像显示

```python
plt.imshow(img, cmap='gist_earth', interpolation='nearest')
# cmap: gist_heat, gist_gray, gray, hot, jet
```

 

### 9. 将Matplotlib输出的结果保存成和原图一样大小的图片

```python
img_np = np.array(img).astype(np.float)[:,:,0]
img_np /= 255.0
img_plt = plt.imshow(img_np)
im_heatmap = np.uint8(img_plt.get_cmap()(img_plt.get_array()) * 255)
```



### 10. 自定义配色

```python
import matplotlib.pyplot as plt
import matplotlib as mpl

_tab20bplus= [
    '#272822',
    '#393b79', '#5254a3', '#6b6ecf', '#9c9ede', '#637939',
    '#8ca252', '#b5cf6b', '#cedb9c', '#8c6d31', '#bd9e39',
    '#e7ba52', '#e7cb94', '#843c39', '#ad494a', '#d6616b',
    '#e7969c', '#7b4173', '#a55194', '#ce6dbd', '#de9ed6',
]

def tab20bplus():
    # return mpl.colors.LinearSegmentedColormap.\
    #     from_list(
    #     'cmap', _tab20bplus, 21) # 插值成21个，会出现颜色区分度低的情况，不适合于表达离散的类别
    return mpl.colors.ListedColormap(_tab20bplus, 'indexed')

color_result = plt.imshow(result, cmap=tab20bplus())
```



### 11. 绘图参数设置

绘图**默认参数**通常保存在以下目录文件中：`plot/Lib/site-packages/matplotlib/mpl-data/matplotlibrc`，代码中需要进行设置时，可以参考这里面的参数。

```python
import matplotlib.pyplot as plt
import matplotlib.pylab as pylab
# 主要修改参数的方法主要有以下几种
# ------- 方法1：适合于批量修改参数 -------
params = {
            'axes.labelsize': '16',
            'xtick.labelsize': '16',
            'ytick.labelsize': '13',
            'lines.linewidth': '2',
            'legend.fontsize': '20',
            'figure.figsize': '26, 24'  # set figure size
        }
pylab.rcParams.update(params)
# ------- 方法2：适合于修改少量参数 -------
plt.rcParams['figure.figsize'] = (8.0, 4.0)
```



### 参考

1.  [matplotlib绘图总结](http://whuhan2013.github.io/blog/2016/09/16/python­-matplotlib­-learn/)