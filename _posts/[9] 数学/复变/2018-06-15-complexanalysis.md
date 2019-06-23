---
layout: post
title: 【复变函数1】极限、微积分、解析
categories:
tags: 9A_代数与分析
keywords:
description:
order: 92501
---

## 一些性质
$Re(z)=\dfrac{z+\bar z}{2},Im(z)=\dfrac{z-\bar z}{2}$  
幅角$\arg z$有无穷多个，主值$-\pi<Arg z\leq \pi$  

指数表达式$\rho e^{i\theta}=\rho(\cos \theta+i\sin\theta)$  


$z*\rho e^{i\theta}$   
实际上是拉伸了 $\rho$ ,然后旋转了 $\theta$  


**TH**  
两个复数的积的模等于模的积，两个复数积的幅角等于幅角的和  
$|x+y|\leq|x|+|y|$  


argument 的性质
- $\arg(\bar z)=-\arg z$
- $\arg(1/z)=-\arg z$
- $\arg(z_1z_2)=\arg z_1 \arg z_2$

## 平面点集
邻域
:    $\mid z-z_0\mid<\delta$的z的集合叫做$z_0$的邻域

内点
:    $z_0\in E,z_0$的某个邻域是$E$的子集，那么$z_0$是$E$的 **内点**  

开集
:    如果$E$的每一个点都是内点，那么称E为 **开集**  

边界点
:    $z_0$的任意邻域内既有属于E的点，也有不属于E的点，称$x_0$是E的 **边界点**

边界
:    全部 **边界点** 的集合  

连通的
:    如果E内任意两点，都可以用折线连接起来，且折线上的点都属于E，那么称E为 **连通的**

开区域
:    连通的开集称为 **开区域**

闭区域
:    开区域与其边界的并集，称为 **闭区域**  


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


## 极限与导数
### 极限
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

### 连续
$\lim\limits_{z\to z_0}f(z)=f(z_0)$叫做在$z_0$ **连续**  
$f(z)$在一个区域D内处处连续，叫做在D内连续  

**TH**  
连续函数的和、差、积、商、复合都连续  


$\lim\limits_{z\to z_0}\dfrac{f(z)-f(z_0)}{z-z_0}$存在，则称为 **可导**，记为$f'(z_0)$  


**TH**  
$[f(z)\pm g(z)]'=f'(z)\pm g'(z)$  
$[f(z)\cdot g(z)]'=f'(z)\cdot g'(z)$  
$[\dfrac{f(z)}{g(z)}]'=\dfrac{f'(z)g(z)-f(z)g'(z)}{g^2(z)}$  

**TH**  
前提$f(z)=u(x,y)+iv(x,y)$在$z_0$有聚点  
$f(z)$有极限的 **充分必要条件** 是$u,v$有极限  
$f(z)$有连续的 **充分必要条件** 是$u,v$连续  
$f(z)$可导的 **充分必要条件** 是$u(x,y),v(x,y)$可导，并且$\dfrac{\partial u}{\partial x}=\dfrac{\partial v}{\partial y},\dfrac{\partial u}{\partial y}=-\dfrac{\partial v}{\partial x}$  


### 解析
解析定义为在邻域内处处可导
1. 可导未必解析
2. 区域内可导$\Longleftrightarrow$解析
3. 不存在这种情况：只在一点解析，而在其邻域内都不解析


根据上文可导的充要条件和解析的定义，解析的充要条件是：  
在区域D内，$u(x,y),v(x,y)$可导，并且$\dfrac{\partial u}{\partial x}=\dfrac{\partial v}{\partial y},\dfrac{\partial u}{\partial y}=-\dfrac{\partial v}{\partial x}$  


调和函数
:    如果实变函数$h(x,y)$在区域D内具有连续的二阶偏导数，并且满足 **拉普拉斯方程** $h_{xx}(x,y)+h_{yy}(x,y)=0$  
称$h(x,y)$是D内的 **调和函数**。  


**TH**  
如果$f(x,y)=u(x,y)+iv(x,y)$在D内解析，那么$u(x,y),v(x,y)$都是 **调和函数**。  


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










--------------------------------------------------------------


## 直线方程

- $z_1\cdot\bar z_2=z_2\cdot\bar z_1=x_1\cdot x_2+y_1\cdot y_2$
 这是一种內积的表示形式

-  $real(B*\bar z)=-C/2$
 其中B是法向量，C是某一个实数 $-\frac{C}{2|B|}$是O点到直线的距离

- $\overline Bz+B\overline z+C=0$
（等价写法）

## 圆的方程
- $|z-z_0|=R$
其中R是半径， $z_0$ 是圆心

- $Az\overline z+\overline B z+B \overline z +C=0$  
其中，A, C是实数  
其中-B/A是圆心，$B \overline B-C$ 是半径。
若A=0变成直线。  


## 参考资料
李红：《复变函数与积分变换》高等教育出版社  
“十五”国家规划教材《复变函数与积分变换》高等教育出版社  
钟玉泉：《复变函数论》高等教育出版社  
