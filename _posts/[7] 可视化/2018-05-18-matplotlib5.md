---
layout: post
title: 【Python】【matplotlib】img
categories:
tags: 7可视化
keywords:
description:
order: 741
---

## 读
```py
import matplotlib.pyplot as plt
a=plt.imread('image.jpg')
# a是一个m*n*3 的 np.array 对象

```

图像数组可以是以下类型
- (1) M*N      此时数组必须为浮点型，其中值为该坐标的灰度；
- (2) M*N*3  RGB（浮点型或者unit8类型）
- (3) M*N*4  RGBA（浮点型或者unit8类型）


## 显

```py
from scipy.stats import uniform
rv=uniform()
b=rv.rvs(size=(400,400,3))
plt.imshow(b)
plt.show()
```
