---
layout: post
title: 【探索】曲面上的均匀采样（随机点）
categories:
tags: A蒙特卡洛方法
keywords:
description:
order: 10003
---

## 描述

在随机模拟实验中，在一个给定的曲面上生成均匀随机点是一个常见的需求。  
但是还没发现有太好通用的算法，这里进行一些探索。  


### 定义

我们把问题分解成两个：
1. 在n维空间（n>=2）上的一条曲线上生成均匀随机点
2. 在n维空间（n>=3）上的一条曲面上生成均匀随机点



#### 何为随机？
我们已经知道 **同余发生器** 或者 **混沌迭代式** 都可以生成伪随机数，这里的随机的定义保持一致。
#### 何为均匀随机？
我们把均匀随机定义为某种度量上的随机：
- 把曲线上的均匀随机，定义为沿着曲线的长度均匀随机采样。
- 把曲面上的均匀随机，定义为沿着曲面上的面积均匀采样。

下面这个就不能定义为随机
[random_surface1]()


#### 为何不涉及其它维度？
因为都是已经解决的问题。  


维度为1的曲线上的均匀分布，实际上就是一般说的均匀分布 `np.random.rand()` 已经有大量的讨论了。


维度为2的曲面上的均匀分布，实际上就是平面上某一块区域，以二维圆面为例，算法如下：

```python
def random_surface():
    while True:
        x, y = np.random.rand(2) * 2 - 1
        if x ** 2 + y ** 2 <= 1:
            return x, y


sample = np.array([random_surface() for i in range(1000)])
plt.plot(sample[:, 0], sample[:, 1], '.')
```

[random_surface2]()


## 曲线上的均匀随机
假设曲线的表示是:  
$$x=\phi(t),y=\psi(t), t\in [a,b]$$  

第一类曲线积分 $\int_{a}^{b} ds$ 就是曲线的长度，  
我们找到x，使得 $\dfrac{\int_{a}^{x} ds}{\int_{a}^{b} ds}=r$，其中$r\in[0,1]$  
也就是$\dfrac{\int_{a}^{x} \sqrt{x'^2+y'^2} dt}{\int_{a}^{b} \sqrt{x'^2+y'^2} dt}=r, r\in[0,1]$

算法就是根据上面的方程，找到$x=f(r)$，然后生成随机的r，进而就找到x了。  

### 案例：圆
$x=\cos t+1,y=\sin t, t\in [0,\pi]$  
$\int_0^t ds = \int_0^t \sqrt{x'^2+y'^2} dt= t$  

方程是 $t/\pi =r$  

代码就有了：
```python
def random_surface():
    r = np.random.rand()
    t = r * np.pi
    x, y = np.cos(t) + 1, np.sin(t)
    return x, y


sample = np.array([random_surface() for i in range(200)])
plt.plot(sample[:, 0], sample[:, 1], '.')
```

[random_surface3]()


### 缺点与方案2
以阿基米德螺旋线$x=t\cos t, y=t\sin t$为例，  
$\int_0^t ds = \int_0^t \sqrt{x'^2+y'^2} dt$  
$= \int_0^t \sqrt{1+t^2}dt$  
$=t/2\sqrt{1+t^2}+1/2 \ln (t+\sqrt{1+t^2})$  


求积分那一步就已经有点计算量了，  
解方程 $t/2\sqrt{1+t^2}+1/2 \ln (t+\sqrt{1+t^2})=r$ 更是几乎不可能。  
因此必须找到一个数值方法。  


```py
import scipy.optimize as opt
import numpy as np


def func(t):
    # 被积部分
    return np.sqrt(1 + np.square(t))


from scipy import integrate

s, _ = integrate.quad(func, 0, 4 * np.pi)  # 分母

result = []
for i in range(300):
    # r = np.random.rand()
    r = i / 300
    t = opt.fsolve(lambda t: integrate.quad(func, 0, t)[0] / s - r, [0], fprime=func)
    x = t * np.cos(t)
    y = t * np.sin(t)
    result.append([x, y])

result = np.array(result)

plt.plot(result[:, 0], result[:, 1], '.')
plt.show()
```
- 手算出被积函数的解析形式（相对来说，不很难）
- 使用了scipy的积分算法、解方程算法（因此效率会变低）
- 为了展示是否均匀，r用均匀值，而不是随机值。在实战中改成随机值。
- fprime 是雅克比矩阵，用于解方程时，加快迭代速度，可以省略。根据Lebniz定理，雅克比矩阵正好等于被积函数。


[4]()


### 方案3

观察到这样的事实，如果把 $f(t)=\sqrt{x'^2+y'^2}$（还需要归一化） 看成一个概率密度函数，那么就可以用蒙特卡洛方法来做了。  


算法步骤：
1. 计算解析形式$f(t)=\sqrt{x'^2+y'^2} dt$
2. 计算$m=\max f(t)$ （或者m比最大值大一些也ok）
2. 在t可能的区域内随机选取一点$t$，计算$f(t)/m$
3. 生成一个随机数$r\in [0,1]$，如果$f(x)/m>r$，则保留x，否则丢弃x并回到2


代码
```python
import scipy.optimize as opt
import numpy as np


def func(t):
    # 被积部分
    return np.sqrt(1 + np.square(t))


left, right = 0, 4 * np.pi
m = -opt.minimize(fun=lambda t: -func(t), x0=np.array([0.1]),
                  method=None, jac=None,
                  bounds=((left, right),)).fun[0]

t_list = []
for i in range(1000):
    t = np.random.rand() * (right - left) + left
    if func(t) / m > np.random.rand():
        t_list.append(t)

x = t_list * np.cos(t_list)
y = t_list * np.sin(t_list)
plt.plot(x, y, '.')
plt.show()
```

优缺点：
- 不用解积分，也不用解方程。大大提高效率
- 但是会有大量空运算（丢弃），所以当f不均匀，或者返回太大时，效率变低。不过貌似效率再低也比方案2强。


这是目前能想到的最有效的方案了。


## 曲面上的均匀随机
### 推广
回到曲线上均匀分布的推导过程，我们找到方程 $\dfrac{\int_{a}^{x} ds}{\int_{a}^{b} ds}=r$，其中$r\in[0,1]$ 的解，作为随机生成算法。  
由于积分和方程难以解出，求助了数值方法去计算。  


对于曲面上的情况，类似的，  
$\iint_D 1d\sigma=\iint_{\Omega_{xy}}\sqrt{1+(\dfrac{\partial z}{\partial x})^2+(\dfrac{\partial z}{\partial y})^2}dxdy=r$  


## 案例：球面
$z=\sqrt{1-x^2-y^2}$  

$f(x,y)=\sqrt{1+(\dfrac{\partial z}{\partial x})^2+(\dfrac{\partial z}{\partial y})^2}=\sqrt{1/(1-x^2-y^2)}$  


为了符号统一，我们把$f(x,y)$定义在$R^2$上，并且对于原本没有定义的点，定义$f(x,y)=0$  
不影响积分$\int_{-\infty}^{+\infty}\int_{-\infty}^{+\infty} f(x,y)dxdy$  

又记 $g(x)=\int_{-\infty}^{+\infty} f(x,y)dy$  
得到$\int_{-\infty}^{x}g(x)/\int_{-\infty}^{+\infty}g(x)=r$，并求解$x=x'$  
$\int_{-\infty}^{y} f(x',y)dy/\int_{-\infty}^{+\infty} f(x',y)dy=r'$，求解$y=y'$  


之后得到$(x',y')$就是我们要的结果








end
