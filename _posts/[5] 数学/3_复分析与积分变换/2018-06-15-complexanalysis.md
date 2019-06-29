---
layout: post
title: 【复变函数1】极限、微积分、解析
categories:
tags: 5_3_复分析与积分变换
keywords:
description:
order: 92501
---

## 概念
### 一些性质
$Re(z)=\dfrac{z+\bar z}{2},Im(z)=\dfrac{z-\bar z}{2}$  
幅角$\arg z$有无穷多个，主值$-\pi<Arg z\leq \pi$  

指数表达式$\rho e^{i\theta}=\rho(\cos \theta+i\sin\theta)$  


$z*\rho e^{i\theta}$   
实际上是拉伸了 $\rho$ ,然后旋转了 $\theta$  


**TH**  
两个复数的积的模等于模的积，两个复数积的幅角等于幅角的和  
$|x+y|\leq|x|+|y|$  


argument 的性质:
- $\arg(\bar z)=-\arg z$
- $\arg(1/z)=-\arg z$
- $\arg(z_1z_2)=\arg z_1 \arg z_2$

### Topology in the Plane
#### 平面点集
邻域
:    $\mid z-z_0\mid<\delta$的z的集合叫做$z_0$的邻域

内点
:    $z_0\in E,z_0$的某个邻域是$E$的子集，那么$z_0$是$E$的 **内点**  

边界点
:    $z_0$的任意邻域内既有属于E的点，也有不属于E的点，称$x_0$是E的 **边界点**

边界
:    全部 **边界点** 的集合  

开集
:    如果$E$的每一个点都是内点，那么称E为 **开集**  

闭集
:    一个集合包含所有边界点，叫做闭集。

连通的
:    如果E内任意两点，都可以用折线连接起来，且折线上的点都属于E，那么称E为 **连通的**

开区域
:    连通的开集称为 **开区域**

闭区域
:    开区域与其边界的并集，称为 **闭区域**  


*closure* of E is $\bar E=E\cup \partial E$  
*interior* of E $E^o$ 表示E所有的内点  

connected(连通的，上面给出一个定义了，这里还有一个等价定义)
:    Two sets $X, Y \in \mathbb C$ are separated if there are disjoint open set $U, V$ so that $X \subset U$ and $Y \subset V$. A set W in $\mathbb C$ is connected if it is impossible to find two separated non-empty sets whose union equals W.



#### 直线方程

- $z_1\cdot\bar z_2=z_2\cdot\bar z_1=x_1\cdot x_2+y_1\cdot y_2$
 这是一种內积的表示形式

-  $real(B*\bar z)=-C/2$
 其中B是法向量，C是某一个实数 $-\frac{C}{2|B|}$是O点到直线的距离

- $\overline Bz+B\overline z+C=0$
（等价写法）

#### 圆的方程
- $|z-z_0|=R$
其中R是半径， $z_0$ 是圆心

- $Az\overline z+\overline B z+B \overline z +C=0$  
其中，A, C是实数  
其中-B/A是圆心，$B \overline B-C$ 是半径。
若A=0变成直线。  







## 收敛序列

Julia sets for quadratic polynomials
Mandelbrot set

复数序列收敛的定义与数学分析中的一致：  
A sequence $\{ s_n \}$ of complex numbers converges to $s \in \mathbb C$ if for every $\varepsilon>0$ there exists $N\geq 1$ such that $\mid s_n - s \mid <\varepsilon$ for all $n\geq N$.  
in this case we write $\lim\limits_{n\to\infty} s_n =s$


复数序列收敛的案例和性质也与数学分析中的一致（省略）  
收敛序列的加减乘除性质与数学分析中的一致（省略）

定理
- $\lim\limits_{n\to\infty} s_n=0 \Longleftrightarrow \lim\limits_{n\to\infty} \mid s_n \mid=0$
- $\lim\limits_{n\to\infty} x_n+iy_n=x+iy \Longleftrightarrow \lim\limits_{n\to\infty} x_n =x,\lim\limits_{n\to\infty} y_n =y$
- 夹逼准则
- 单调有界

### julia set

我们讨论这个迭代式 $f(z)=z^2+c$  

#### 问题1
为什么不用更一般化的迭代式呢 $p(z)=az^2+bz+d$？  
令$\phi(z)=az+b/2, c=ad+b/2+(b/2)^2$，有$\phi(p(z))=f(\phi(z))$  

也就是，有下面这个图。  


![abc](https://i.imgur.com/Uj9Dtjh.jpg)

进一步的，$p=\phi^{-1} \circ f \circ \phi $  
再进一步，$p^{n \circ } = \phi^{-1} \circ f^{n\circ} \circ \phi $  

结论是，我们只需要研究 $f(z)=z^2+c$ 就可以了

(本节$f^n$表示 $f^{n\circ}$)  
#### 定义
Julia set 是x的这样一个集合，其邻域对迭代表现出混沌性。  
相反，Fatou set 是x的这样一个集合，其邻域对迭代不表现出混沌性。  
Julia set 和 Fatou set 是补集  


**例子**  
以$f(z)=z^2$为例，对应的 julia set 是 $$\{ z\mid \mid z \mid =1\}$$，对应的 Fatou set 是补集。  


定义$$A(\infty)=\{z: f^n(z) \to\infty \}$$，那么这个集合有如下性质：  
- $A(\infty)$ is open, connected, unbounded
- $A(\infty)$是 Fatou set 的子集
- $A(\infty)$的边界是 Julia set

推论：
- Fatou set is open and unbounded
- Julia set is closed and bounded set
- $J(f) \cap F(f) = \varnothing$
- completely invariant $f(J)=J, f(F)=F$


**例子** $f(z)=z^2-2$  
引入$\phi(w)=w+1/w$,就有$z^2=\phi^{-1} \circ f \circ \phi (z)$  
可以找到$A(\infty)$, 进而找到边界 Julia set is $[-2,2]$  
（过程不难，但有点绕，可以在纸上画一画）


#### 数值方法
我们有这个定理：  
对于$f(z)=z^2+c, R=\dfrac{1+\sqrt{1+4\mid c\mid}}{2}$  
如果$\mid z_0 \mid >R$，那么$z_0 \in A(\infty)$ （也就是说$\lim\limits_{n\to \infty} f^{n\circ}(z_0)=\infty$）  


所以，$\exists n, f^{n\circ}(z_0)>R \Longrightarrow z_0 \in A(\infty)$  
（在迭代过程中，如果任意一次迭代，其值大于R，那么后面的值就趋近于无界）  


这给我们一种用迭代法找 Julia set 的方法：  
在迭代过程中，任意一次迭代值大于 R，就剔除 Julia set，到 max_iter 后画图  
进一步，根据第一次出现R的迭代步骤的不同，可以涂上不同的颜色。  


```py
import numpy as np
import matplotlib.pyplot as plt

c = -0.75 + 0.2j
R = (1 + np.sqrt(1 + 4 * abs(c))) / 2

n_grid = 1000
Z = np.linspace(-2, 2, n_grid).reshape(1, n_grid) + np.linspace(2, -2, num=n_grid).reshape(n_grid, 1) * 1j
Julia_set = np.zeros_like(Z)
max_iter = 50
for i in range(max_iter):
    Z = np.where(Julia_set == 0, np.square(Z) + c, 2 * R)  # 已归入 Julia set 的点，记为大数，之后不再参与计算
    Julia_set = np.where((Julia_set == 0) & (np.abs(Z) > R), i, Julia_set)
    # 第一次归入Julia set，记入其迭代次数，如果不想画彩图，把i改为1

Julia_set = np.where(Julia_set == 0, 2 * max_iter, Julia_set)  # 到最后都没有剔除的点，赋值为 2*max_iter
plt.imshow(np.abs(Julia_set))
plt.show()
```

### Mandelbrot set
c的集合，使得 $J(f)$是连通集。  
精确的定义：$$M=\{c\in \mathbb C: J(z^2+c) \mathrm{\ is \ connected} \}$$  


例如，$0, -2, 0.25 \in M, 1\not \in M$

**TH1**  
$J(f) \mathrm{\ is \ connected} \Longleftrightarrow 0\not \in A(\infty)$

**TH2**  
$c\in M \Longleftrightarrow \mid f^{n\circ} \mid \leq 2, \forall n\geq 1$






```py
import numpy as np
import matplotlib.pyplot as plt

n_grid = 1000
c = np.linspace(-2, 2, n_grid).reshape(1, n_grid) + 1j * np.linspace(2, -2, n_grid).reshape(n_grid, 1)
Mandelbrot = np.zeros_like(c)
f_z = 0
max_iter = 30

for i in range(max_iter):
    f_z = np.where(Mandelbrot == 0, np.square(f_z) + c, 1e8)  # 已经被归入 Mandelbrot set 的点，记为很大，之后不再参与计算
    Mandelbrot = np.where((Mandelbrot == 0) & (np.abs(f_z) > 2), i, Mandelbrot)  # 记录首次大于2的迭代次数，如果不想画彩图，把 i 换成 1

Mandelbrot = np.where(Mandelbrot == 0, max_iter, Mandelbrot)
plt.imshow(np.abs(Mandelbrot))
plt.show()
```
（强烈建议跑一下这段代码，缩小c的范围，同时扩大迭代次数）

性质：
- M is connected
- $M\subset B_2(0)$
- M的边界点叫做 Misiurewicz points，其性质是 $f^{n\circ}(0)$ is pre-periodic, but not periodic  
（$c=i$就是一个 Misiurewicz point，以此为例形象化说明，根据定义，c使Julia set介于连通集和非连通集之间，所以Julia set更像一条线，无限放大这条线，看起来就像分形曲线。与此同时，因为Mandelbrot的稠密和花纹性质，在i周围无限放大，也得到分形曲线）
- Misiurewicz points 这条边界线稠密

**Big Open Conjecture**  
The Mandelbrot set is locally connected, that is, for every $c\in M$ and every open set V with $c\in v$, there exists an open set U such that $c\in U\subset V$ and $U\cap M$ is connected.


## 极限
**定义**  
$\forall \varepsilon>0,\exists \delta(\varepsilon)$使得$\forall z,0<\mid z-z_0\mid<\delta$，都满足$\mid f(z) -A\mid<\varepsilon$  
A就是$f(z)$在$z\to z_0$的极限，记做$\lim\limits_{z\to z_0}f(z)=A$  


**TH**  
若$\lim\limits_{z\to z_0}f(z)=A$，那么$\lim\limits_{z\to z_0}\mid f(z)\mid=\mid A\mid$  





**补充定义**  
$\lim\limits_{z\to \infty}f(z)=A,\lim\limits_{z\to z_0}f(z)=\infty,\lim\limits_{z\to \infty}f(z)=\infty$  
沿用原定义，并且注意取到无穷大的所有邻域，而不是沿某个方向趋近。  


**TH**  
若$\lim\limits_{z\to z_0}f(z)=A,\lim\limits_{z\to z_0}g(z)=B$  
那么，  
$\lim\limits_{z\to z_0} [f(z) \pm g(z)]=A\pm B$  
$\lim\limits_{z\to z_0} [f(z) \cdot g(z)]=A\cdot B$  
$\lim\limits_{z\to z_0} [f(z) / g(z)]=A/ B ,(B\neq 0)$  

## 连续
$\lim\limits_{z\to z_0}f(z)=f(z_0)$叫做在$z_0$ **连续**  
$f(z)$在一个区域D内处处连续，叫做在D内连续  

**TH**  
连续函数的和、差、积、商、复合都连续  

## 微分
$\lim\limits_{z\to z_0}\dfrac{f(z)-f(z_0)}{z-z_0}$存在，则称为 **可导**，记为$f'(z_0)$  


**TH**  
$[f(z)\pm g(z)]'=f'(z)\pm g'(z)$  
$[f(z)\cdot g(z)]'=f'(z)\cdot g'(z)$  
$[\dfrac{f(z)}{g(z)}]'=\dfrac{f'(z)g(z)-f(z)g'(z)}{g^2(z)}$  
$\dfrac{d f(g(z))}{dz}=f'(g(z))g'(z)$  

**TH**  
前提$f(z)=u(x,y)+iv(x,y)$在$z_0$有聚点  
$f(z)$有极限的 **充分必要条件** 是$u,v$有极限  
$f(z)$有连续的 **充分必要条件** 是$u,v$连续  
$f(z)$可导的 **充分必要条件** 是$u(x,y),v(x,y)$可导，并且$\dfrac{\partial u}{\partial x}=\dfrac{\partial v}{\partial y},\dfrac{\partial u}{\partial y}=-\dfrac{\partial v}{\partial x}$（Cauchy-Riemann Equations, also $f'(z_0)=f_x(z_0)=-if_y(z_0)$）  
（用两个方向的方向导数证明）  


### 常见微分
$f(z)=z^n,f'(z)=nz^{n-1}$  
...


## 解析
解析定义为在邻域内处处可导
1. 可导未必解析
2. 区域内可导$\Longleftrightarrow$解析
3. 不存在这种情况：只在一点解析，而在其邻域内都不解析


根据上文可导的充要条件和解析的定义，解析的充要条件是：  
在区域D内，$u(x,y),v(x,y)$可导，并且其一阶偏导数连续，并且$\dfrac{\partial u}{\partial x}=\dfrac{\partial v}{\partial y},\dfrac{\partial u}{\partial y}=-\dfrac{\partial v}{\partial x}$  


调和函数
:    如果实变函数$h(x,y)$在区域D内具有连续的二阶偏导数，并且满足 **拉普拉斯方程** $h_{xx}(x,y)+h_{yy}(x,y)=0$  
称$h(x,y)$是D内的 **调和函数**。  


**TH**  
如果$f(x,y)=u(x,y)+iv(x,y)$在D内解析，那么$u(x,y),v(x,y)$都是 **调和函数**。  

### 例子
- 多项式函数一定解析
- 有理函数（两多项式函数的商）在定义域内解析
- $f(z)=Re z,f(z)=Im z$都处处不解析
- $f(z)=\mid z\mid$在非0处不可导，在0处可导。所以处处不解析。



## 积分
$S_n=\sum f(\xi_k)\Delta z_k$,记为$\int_C f(z)dz$  
$\int_Cf(z)dz=\int_C(u+vi)(dx+idy)=\int_C udx-vdy+i\int_Cvdx+udy$  

**TH**  
C是一条闭曲线，$f(z)$在以C为边界的有界闭区域D上解析，那么  
$\int_C f(z)dz=0$  
**推论**  
$f(x)$在单连通区域D内解析，AB两点之间两条路径$L_1,L_2\subset D$，有  
$\int_{C_1}f(z)dz=\int_{C_2}f(x)dz$  


**TH**（柯西积分公式）  
$f(z)$在简单闭曲线C所围成的区域D内解析，在$D\cup C$上连续，$z_0$是D内任意一点，则  
$f(z_0)=\dfrac{1}{2\pi i}\oint_C\dfrac{f(z)}{z-z_0}dz$  
**推论1**（平均值公式）  
$f(z)$在$\mid z-z_0\mid<R$内解析，在$\mid z-z_0\mid=R$内连续，那么  
$f(z)=\dfrac{1}{\pi}\int_0^{2\pi}f(z_0+Re^{i\theta})d\theta$  
**推论2**  
$f(z_0)$在由闭曲线$C_1,C_2$围城的二连域内解析，并在$C_1,C_2$上连续，$C_2$在$C_1$内部，$Z_0$为D内一点，则  
$f(z_0)=\dfrac{1}{2\pi i}\oint_{C_1}\dfrac{f(z)}{z-z_0}dz-\dfrac{1}{2\pi i}\oint_{C_2}\dfrac{f(z)}{z-z_0}dz$  







## 解析延拓
**指数函数**  
$e^{x+iy}=e^x(\cos y+i\sin y)$  


**对数函数**  
$Ln z=\ln \mid z\mid +i \arg z=\ln \mid z\mid +i Arg z +2k\pi$  
也可以记为:  
$\ln z=\ln \mid z\mid +iArg z$  
$Ln z=\ln z+ 2k\pi i$  
注意$Ln z^n \neq n Ln z$


**幂函数**  
$z^a=e^{a Ln z}=e^{a\ln z}e^{2k\pi ia}$  
1. 当$a$是整数时，只有1中可能取值
2. 当$a$是有理数$q/p$时，有p种可能值
3. 当$a$是无理数或复数时，有无穷多个值


($2^{\sqrt 2}$有1个实数值和无穷个复数值)


**三角函数**  
$e^{ix}=\cos x+i\sin x,e^{-ix}=\cos x-i\sin x$,可以得到  
$\sin x=\dfrac{e^{ix}-e^{-ix}}{2i},\cos x=\dfrac{e^{ix}+e^{-ix}}{2}$  





--------------------------------------------------------------


## 参考资料
coursera:[Introduction to Complex Analysis](https://www.coursera.org/learn/complex-analysis/)  
李红：《复变函数与积分变换》高等教育出版社  
“十五”国家规划教材《复变函数与积分变换》高等教育出版社  
钟玉泉：《复变函数论》高等教育出版社  
