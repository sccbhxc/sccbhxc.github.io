---
layout: post
title: matplotlib绘图设置
category: 编程
tags: Programming
description: matplotlib
---



### 1. 绘图参数设置

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



### 2. 设置字体

```python
import matplotlib
myfont = matplotlib.font_manager.FontProperties(fname=r'C:/Windows/Fonts/msyh.ttf') 
```

