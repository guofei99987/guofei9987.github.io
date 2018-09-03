---
layout: post
title: 【复变函数3】共形映射
categories:
tags: 9A_代数与分析
keywords:
description:
order: 92503
---


## 基本定义
$w=f(z)$把曲线C映射到曲线$\Gamma,z_0\in C,w_0\in \Gamma,w_0=f(z_0)$  
把领域映射到邻域$z=z_0+\mid\Delta z\mid e^{i\theta},w=w_0+\mid\Delta w\mid e^{i\phi}$  
如果$\lim\limits_{z\to z_0}\dfrac{\mid w-w_0 \mid}{\mid w-w_0 \mid}$存在，那么这个极限值叫做曲线C经函数$w=f(z)$映射后在$z_0$处的 **伸缩率**  
如果$\lim\limits_{z\to z_0}(\phi-\theta)$存在，那么这个极限值叫做曲线C经函数$w=f(z)$映射后在$z_0$处的 **旋转角**  


如果$w=f(z)$在D内解析，那么$f'(z_0)=\lim\limits_{\Delta z\to 0}\dfrac{\Delta w}{\Delta z}=\lim\limits_{\Delta z\to 0}\dfrac{\mid\Delta w\mid}{\mid \Delta z\mid }e^{i(\phi-theta)}$  
$\lim\limits_{\Delta z\to 0}\dfrac{\mid\Delta w\mid}{\mid \Delta z\mid }=\mid f'(z_0)\mid$，也就是说，对于任何过$z_0$的曲线C，伸缩率都不变，这种性质叫做 **伸缩率不变性**  
$\lim\limits_{\Delta z\to 0}(\phi-\theta)=\arg f'(z_0)$，也就是说，对于任何过$z_0$的曲线C，旋转角都不变，这种性质叫做 **旋转角不变性**  
（$f'(z_0)\neq 0$是必要的，否则保角性将不成立）  


## 共形映射
对于D内的映射$w=f(z)$,如果它在任一点都具有 **伸缩率不变性** 和 **旋转角不变性** 称$w=f(z)$是 **第一类保角映射**  
如果伸缩率不变，保持角度不变但方向相反，称为 **第二类保角映射**  
（根据上一部分的分析，如果$w=f(z)$在D内解析，那么一定在D内是保角映射）  


共形映射
:    $w=f(z)$是区域D内的 **第一类保角映射**，并且当$z_1\neq z_2$，有$f(z_1)\neq f(z_2)$  


## 分式线性映射
$w=\dfrac{az+b}{cz+d} , (ad-bc\neq 0)$  
分式线性映射可以分解成以下4种简单函数
1. $w=z+b$（b是复数），是平移映射
2. $w=ze^{i\theta},(\theta\in R)$,是旋转映射
3. $w=rz,(r>0,r\in R)$,是相似映射（把曲线放大）
4. $w=\dfrac{1}{z}$,是反演映射，（可以理解为两个点关于圆对称）


（为了形象理解反演映射，写了个动画程序,可以自行运行试试看）
```py
# 复变函数1/z的形象理解
import numpy as np
import matplotlib.pyplot as plt


def func(z):
    w = 1 / z
    return w


fig = plt.figure(figsize=(5, 5))
ax = fig.add_subplot(111)
z = 0.1 + 0j
w = func(z)

line1 = ax.plot(z.real, z.imag, 'bo')
line2 = ax.plot(w.real, w.imag, 'ro')

theta_circle = np.arange(0, 2 * np.pi, 0.01)
line3 = ax.plot(np.cos(theta_circle), np.sin(theta_circle))

ax.set_xlim(-1, 5)
ax.set_ylim(-1, 5)
plt.ion()  # 第一个重要的点
p = plt.show()  # 第二个重要的点

for i in range(1000):
    z = i / 100 - int(i / 100) + 0.5 + 0j
    w = func(z)
    plt.setp(line1, 'xdata', z.real, 'ydata', z.imag)  # 第三个重要的点
    plt.setp(line2, 'xdata', w.real, 'ydata', w.imag)  # 第三个重要的点
    plt.pause(0.1)  # 第四个重要的点
```


**TH1**  
分式线性映射是共形映射

### 分式线性映射的保圆性
所谓保圆性，是把圆映射到圆。（可以把直线也看成是无穷大的圆）  
显然，平移映射、旋转映射有保圆性，相似映射和反演映射的保圆性直观理解如下：  
```py
# 保圆性的直观理解
import numpy as np
import matplotlib.pyplot as plt


def func(z):
    # w = 2 * z # 相似映射
    w = 1 / z # 反演映射
    return w


fig = plt.figure(figsize=(5, 5))
ax = fig.add_subplot(111)
theta_circle = np.arange(0, 6.31, 0.1)
z_list=np.exp(1j*theta_circle)*0.3+0.5
w_list=func(z_list)
circle_list=np.exp(1j*theta_circle)

line1=ax.plot(z_list.real,z_list.imag,'.')
line2=ax.plot(w_list.real,w_list.imag,'.')
line3 = ax.plot(circle_list.real, circle_list.imag)
ax.set_xlim(-1, 5)
ax.set_ylim(-3, 3)
p = plt.show()
```
![complexanalysis](https://github.com/guofei9987/StatisticsBlog/blob/master/%E9%99%84%E4%BB%B6/complexanalysis/complexanalysis.png?raw=true)








## 参考资料
李红：《复变函数与积分变换》高等教育出版社  
“十五”国家规划教材《复变函数与积分变换》高等教育出版社  
钟玉泉：《复变函数论》高等教育出版社  
