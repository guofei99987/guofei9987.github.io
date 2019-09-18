---
layout: post
title: 【随机过程】
categories:
tags: 4_4_随机过程
keywords:
description:
order: 470
---

## 定义
$$\{ \Omega,F,P \}$$是一个概率空间，$\forall t\in T,X_t(w)$是定义在其上取值与$(S,B)$的随机变量，称$\{X_t:t\in T\}$是T上的一个随机过程  
定义中有两个元素：
1. 概率空间
2. 时间集合T，可以是$[0,+\infty),(-\infty,+\infty),N_+$ 等等，甚至未必是实数集。这里主要考虑实数空间或整数空间


## 基本定义
互相关函数$R_{XY}(t_1,t_2)=E[X(t_1)Y(t_2)]$  
互协方差函数$C_{XY}=Cov[X(t_1),Y(t_2)]$  


自相关函数$R_x(t_1,t_2)=E[X(t_1)X(t_2)]$  
均方值函数$\psi_X^2(t)=E[X^2(t)]=R_X(t,t)$  

## 独立性
### 相互独立


相互独立
:    如果$\forall x_i,y_j,t_i,t_j'$,都有$F(x_1,...,x_n,y_1,...,y_m;t_1,...,t_n,t_1',...,t_m')=F_X(x_1,...,x_n,t1,...,t_n)F_Y(y_1,...,y_m,t_1',...,t_m')$  
那么称两个随机过程 **相互独立**  


独立增量过程
:    如果$X(t_1)-X(t_0),...,X(t_n)-X(t_{n-1})$相互独立，称为 **独立增量过程**


齐次独立增量过程
:    $X(t)-X(s)$的分布只与$t-s$有关


定理：$$\{ X(t),t\geq 0\}$$是独立增量过程，且$X(0)=0$,那么$C_X(s,t)=\sigma_X^2[\min(s,t)]$  


泊松过程
:    满足3条：
1. 是独立增量过程
2. $\forall 0\leq t_0<t,N(t_0,t)\sim\pi(\lambda(t-t_0))$  
3. $N(0)=0$


维纳过程
:    满足一下3条:
1. 是独立增量过程
2. $\forall 0\leq t_0<t,w(t)-w(t_0)\sim N(0,\sigma^2(t-s))$  
3. $w(0)=0$


定理：  
$$\{ w(t),t\geq 0\}$$是维纳过程，那么
1. $\{ w(t)\}$是正态过程
2. $w_1(t)=Cw(\dfrac{t}{C^2})$也是维纳过程  





--------------------------------


## ITO引理的证明
$\Delta s=\dfrac{\partial s}{\partial x} \Delta x
+\dfrac{\partial s}{\partial t} \Delta t+\dfrac{\partial^2 s}{\partial x \partial t} \Delta x \Delta t
+\dfrac{\partial^2 s}{\partial t^2} \Delta t^2$
⇔(前提：s(x,t)高阶连续)
$ds=\dfrac{\partial s}{\partial x} dx + \dfrac{\partial s}{\partial t} dt$

## 泊松过程
### 计数过程
- N(t)≥0
- N(s)≤N(t)

### Def1
- N(0)=0
- N(t)是独立增量过程
- $P\{ N(t+s) - N(t) \} = e^{-\lambda t} \dfrac{(\lambda t)^n}{n!}$

### Def2
- N(0)=0
- N(t)是独立增量过程,并且是平稳过程
- $P\{ N(t+s) - N(t)=1 \} = \lambda h + o(h)$
- $P\{ N(t+s) - N(t) \geq 2 \} = o(h)$
