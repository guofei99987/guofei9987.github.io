---
layout: post
title: law of large numbers
categories:
tags: 4_2_概率论
keywords:
description:
order: 9550
---

## 切比雪夫不等式
（Chebyshev inequality）  
**切比雪夫不等式**  
X的均值是u,方差是$\sigma^2$,那么，$\forall t>0,Pr[\mid X-u \mid \geq t \sigma] \leq \dfrac{1}{t^2}$  
等价表示：  
$Pr[\mid X-u \mid \geq s] \leq \dfrac{\sigma^2}{s^2}$  

证明提要：  
$\sigma^2=\sum\limits_{x_i}(x_i-u)^2f(x_i) \geq \sum\limits_{\mid x_i -u \mid \geq s}(x_i-u)^2f(x_i)$  
$ \geq \sum\limits_{\mid x_i -u\mid \geq s}s^2f(x_i)=s^2\sum\limits_{\mid x_i -u\mid \geq s}f(x_i)$  
$=s^2 Pr[\mid X-u \mid \geq s]$  

**切比雪夫不等式的推广**  
X的均值是u，n阶中心距$u_{\mid n \mid}=E[\mid X-u \mid ^n],(n \geq 1)$,有  
$Pr[\mid X-u \mid \geq t]\leq \dfrac{u_{\mid n\mid}}{t^n}$  

证明方法相同  

**切比雪夫单边不等式**  
X的均值是u,方差是$\sigma^2$,那么，$\forall s>0,Pr[ X-u  \geq s] \leq \dfrac{\sigma^2}{s^2+\sigma^2}$  


## 大数定律

### 弱大数定律

如果$X_i$独立同分布，$u=EX,\bar X=1/n \sum X_i $,  
$\forall \epsilon >0,n\to \infty ,Pr[\mid \bar X-u \mid >\epsilon] \to 0$


### 辛钦大数定律
辛钦大数定律：$$\lim\limits_{n\to \infty} P\{ \mid \dfrac{1}{n} \sum\limits_{k=1}^n X_k -u\mid < \varepsilon \}$$  

### 伯努利大数定理

伯努利大数定律：$$\lim\limits_{n\to \infty} P\{ \mid \dfrac{f_A}{n} -p\mid < \varepsilon \}$$  

## 中心极限定理

中心极限定理：$$\lim\limits_{n\to \infty} P\{ \dfrac{\sum\limits_{i=1}^n X_i -n \mu}{\sqrt n \sigma} \leq x\} =\Phi(x)$$  

也就是说，独立同分布的均值，服从$N(\mu,\sigma^2)$  
