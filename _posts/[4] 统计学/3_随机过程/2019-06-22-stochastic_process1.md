---
layout: post
title: 【随机过程】1
categories:
tags: 4_2_时间序列和随机过程
keywords:
description:
order: 471
---

这篇是 coursera.com 上的 [Stochastic processes](https://www.coursera.org/learn/stochasticprocesses) *by National Research University Higher School of Economics* 的学习笔记

## week1
我们有3种工具
- probability theory
- mathematical statistics
- stochastic processes

### 一个例子
在讲 mathematical statistics 的时候，举了个经典例子：
想估计池塘里多少鱼，第一次捞上来一批，打上标签，捞q次，统计每次捞上来的比例。  
假设：
- 总共N条鱼，这是需要我们估计的量
- 第一次捞上来M条鱼，并打标
- 第k次捞上来$n_k$条鱼，其中打标的有$m_k$条

计算得知，发生的概率为$P (marked=m_k) = \dfrac{C_M^m C_{N-M}^{n-m}}{C_N^n}$  
我们使用最大似然估计 $\max\limits_N \sum\limits_{k=1}^q \ln P (marked=m_k)$ 可以估计出N

### 定义：概率测度
$$\{ \Omega,F,P \}$$ ，其中
- $\Omega$ is sample space
- $F$ is sigma algebra，满足
    - $\Omega\in F$
    - $A\in F\Longrightarrow \Omega - A \in F$
    - $A_1,A_2,...,A_n \in F \Longrightarrow \cup A_i \in F$
- $P$ is probability measure
    - $P\{ \Omega \}=1$
    - $A_i \in F,P(\cup A_i)=\sum P(A_i)$



以扔硬币n次为例，$\Omega$ 中有$2^n$个元素，$F$中有$2^{2^n}$个元素  





Random variable is a function $\xi: \Omega \to \mathbb R$ such that $\forall B \in \mathbb {B(R)}:\xi^{-1}(B) \in F$  

Let us consider the following example. An agent flips a coin 2 times. In this model, $$\Omega={(h,h);(h,t);(t,h);(t,t)}$$, where t means tails and h means heads. σ-algebra $\mathbb F$ contains all possible combinations of those 4 elements in $\Omega$ (by the way, the number of elements in $\mathbb F$ is exactly $2^4$ ).  

Let, $\xi(h;h)=1, \xi(h;t)=2, \xi(t;h)=3, \xi(t;t)=4$. (Note that $$P\{\xi=k\}=14,k=\{1,2,3,4\}$$ and 0 otherwise.) Clearly, if we apply $\xi^{-1}$ to both sides of those equations, we obtain $\xi^{-1}(1)=(h;h), \xi^{-1}(2)=(h;t), \xi^{-1}(3)=(t;h), \xi^{-1}(4)=(t;t)$. Therefore, $\forall B \in \mathbb {B(R)}: \xi^{-1}(B) \subset F$.

### 定义：随机过程
$X:T\times \Omega \to \mathbb R$

这个复习一下
https://www.coursera.org/learn/stochasticprocesses/lecture/mIqkz/week-1-5-trajectories-and-finite-dimensional-distributions

## Renewal process
$S_0=0,S_n=S_{n-1}+\xi_n,$  
$\xi_1,\xi_2,...,i.i.d>0$, as  
$P(\xi_i>0)=1$（等价于$F(0)=0$）

### counting process
$N_t=\arg\max\limits_k (\xi_k\leq t)$
