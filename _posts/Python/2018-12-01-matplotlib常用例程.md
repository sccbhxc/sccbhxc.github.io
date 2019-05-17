---
layout: post
title: matplotlib常用例程
category: 编程
tags: Programming
desciption: 收集比较常用的matplotlib代码片段
---

### 1. Detection中绘制带Label的Bbox

```python
import matplotlib.pyplot as plt
import matplotlib.patches as patches

import cv2
# ----------- 使用opencv绘制Bounding box -----------
def plot_one_box(x, img, color=None, label=None, line_thickness=None):  # Plots one bounding box on image
    tl = line_thickness or round(0.002 * max(img.shape[0:2])) + 1  # line thickness
    color = color or [random.randint(0, 255) for _ in range(3)]
    c1, c2 = (int(x[0]), int(x[1])), (int(x[2]), int(x[3]))
    cv2.rectangle(img, c1, c2, color, thickness=tl)
    if label:
        tf = max(tl - 1, 1)  # font thickness
        t_size = cv2.getTextSize(label, 0, fontScale=tl / 3, thickness=tf)[0]
        c2 = c1[0] + t_size[0], c1[1] - t_size[1] - 3
        cv2.rectangle(img, c1, c2, color, -1)  # filled
        cv2.putText(img, label, (c1[0], c1[1] - 2), 0, tl / 3, [225, 255, 255], thickness=tf, lineType=cv2.LINE_AA)

# -------------- 使用Matplotlib中提供的配色Cmap ------------
cmap = plt.get_cmap('tab20b')
color = cmap[int(np.where(unique_labels == int(cls_pred))[0])]

# -------------- 使用opencv绘制Bounding box ------------
plot_one_box([x1, y1, x2, y2], img, label=label, color=color)


# ----------- 使用matplotlib绘制Bounding box -----------
# Create a Rectangle patch
bbox = patches.Rectangle((x1, y1), box_w, box_h, linewidth=2,
                        edgecolor=color,
                        facecolor='none')
# Add the bbox to the plot
ax.add_patch(bbox)
# Add label
plt.text(x1, y1, s=classes[int(cls_pred)], color='white', verticalalignment='top',
        bbox={'color': color, 'pad': 0})
```



### 2. Segmentation中的Mask

```python
import matplotlib.pyplot as plt
import matplotlib as mpl

# -------------- 根据类别自定义类别对应颜色 --------------
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
color_result = Image.fromarray(np.uint8(
    color_result.get_cmap()
    (color_result.get_array()) * 255
))

color_result.save('outputs/result.png')
```

