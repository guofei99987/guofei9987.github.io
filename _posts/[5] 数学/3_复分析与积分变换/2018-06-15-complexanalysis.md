---
layout: post
title: 【复变函数0】基本概念
categories:
tags: 5_3_复分析与积分变换
keywords:
description:
order: 92500
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


argument 的性质:
- $\arg(\bar z)=-\arg z$
- $\arg(1/z)=-\arg z$
- $\arg(z_1z_2)=\arg z_1 \arg z_2$

## Topology in the Plane
### 平面点集
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



### 直线方程

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





$f'(z)=\dfrac{1}{f'(g(z))},  z\in D$



## 参考资料
coursera:[Introduction to Complex Analysis](https://www.coursera.org/learn/complex-analysis/)  
李红：《复变函数与积分变换》高等教育出版社  
“十五”国家规划教材《复变函数与积分变换》高等教育出版社  
钟玉泉：《复变函数论》高等教育出版社  
