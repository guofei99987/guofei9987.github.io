---
layout: post
title: Practical aspects of Deep Learning
categories:
tags: 2_3_神经网络与TF
keywords:
description:
order: 450
---
*吴恩达的课程笔记*  

## train/dev/test
这个已经很熟悉了。  
dev又称为 development set, hold-out cross-validation set

- 分割问题  
以前在小数据上的分割： 70/30, 60/20/20  
big data: dev/test 占比可以很小，just big enough for you to evaluat。（例如，你有一百万数据，只需要一万数据来做dev/test即可）
- Mismatched train/test distribution  
有时候，你的training set 来自某个来源和dev/test 来自另一个来源。  
**Makesure dev and test come from same distribution**
- Not having a test set might be okay. (Only dev set.)


## Bias/Variance

- high variance : train set 上表现良好，dev set 上表现不佳（例如，train/dev 上的error 是1%/11%）
- high bias ： 在training set 上表现也不佳 (例如，train/dev 上的error 是15%/16%)
- 也可能出现 high bias & variance. 或者 low bias & variance

解决：
- high bias?
    - big network
    - train longer
- high variance?
    - more data
    - regularization
- trade-off between bias & variance

## Regularization

在 logistic regression 中  
$J(w,b)=\dfrac{1}{m}\sum\limits_{i=1}^m\mathcal L(\hat y^{(i)},y^{(i)})+\dfrac{\lambda}{2m}\mid\mid w\mid\mid_2^2$  
- 为什么不加b？因为b只有1个维度，影响很小，可加可不加，为了简便就省了。
- L1-regularization (w will be sparse), L2-regularization
- $\lambda$ 值取决于 trade-off between bias & variance


在 neural network 中
$J(w,b)=\dfrac{1}{m}\sum\limits_{i=1}^m\mathcal L(\hat y^{(i)},y^{(i)})+\dfrac{\lambda}{2m}\sum\limits_{l=1}^L\mid\mid W\mid\mid_F^2$  
- 不是 “L2 norm”，而是“Frobenius norm”，因为里面是矩阵，但意思差不多
- 对back propagation 的影响：$dW^{[l]}=dW^{[l]}_{old}+\dfrac{\lambda}{m}W^{[l]}$. Weight decay: $W^{[l]}:=W^{[l]}-\alpha dW^{[l]}=(1-\dfrac{\lambda}{m}W^{[l]})-\alpha dW^{[l]}_{old}$  


**Why** ?  
其实看过的另一本书里也回答过。  
- 想象极端情况，W接近0，多层神经网络的表现接近1层（logistics regression）
- w接近0，使得激活函数局部更接近线性。

## Dropout Regularization
```py
# 对 l=3 进行dropout 操作
keep_prob=0.8

# forward propagation
d3=np.random.rand(a3.shape[0],a3.shape[1])<keep_prob
a3=a3*d3 # element product
a3/=keep_prob # 保证z的期望不变 “Inverted dropout technique”

# back propagation
da3=da3*d3
da3/=keep_prob
```
- test 阶段不Dropout
- 画loss图所用的数据不Dropout

**Why** ？  
- Cannot rely on any one feature, so have to spread out weights. 实际上产生的是缩小weight的作用，也就是 regularization 的效果
- 用实际上更小的 neural network 来实现 regularizing effect.  

### Other regularization methods
- 用更多数据效果会更好，但是获得新数据很贵，对图像来说，可以这么做：反转、随机缩放、轻微旋转、扭曲
- early stopping. 实际上也是在W太大之前停止迭代，缺点是不太以$J$为优化目标。你可以用L2 regularization，然后就可以多迭代了。


## Normalizing inputs
$\dfrac{x-u}{\sigma^2}$  

**Why** ？
（我觉得更好的解释是，如果不做标准化，且数据分布差异大，那么实际上相当于初始化的权重偏差特别大，需要迭代相当多次才行，而这是浪费）

## Vanishing / Exploding gradients

## Weight Initialization
$Z=W_{1\times n}X+b$，上面的经验中，不能让Z太大，这就要求W要随着n变小。  
可以在初始化时，使$\mathrm{var} w=1/n$  
- ReLU：$\mathrm{var} w=2/n$
- tanh：$1/n$更好或者$2/(n^{[l-1]}+n^{[l]})$


## Gradient checking
积分的数值求法:
- $f'(\theta)=\dfrac{f(\theta+\varepsilon)-f(\theta-\varepsilon)}{2\varepsilon}+O(\varepsilon^2)$  
- $f'(\theta)=\dfrac{f(\theta+\varepsilon)-f(\theta)}{\varepsilon}+O(\varepsilon)$  

Gradient checking可以用来找程序里面的bug  
step1：reshape and concate $W^{[i]},b^{[i]},\forall i$ into $\theta$  
step2: reshape and concate $dW^{[i]},db^{[i]},\forall i$ into $d\theta$  
step3： for each i:$d\theta_{approx}[i]=\dfrac{J(\theta_1,\theta_2,...,theta_i+\varepsilon,...)-J(\theta_1,\theta_2,...,theta_i-\varepsilon,...)}{2\varepsilon}$  
step4: check $d\theta_{approx}==d\theta$。use $\dfrac{\mid\mid d\theta_{approx}-\theta\mid\mid_2}{\mid\mid d\theta_{approx} \mid\mid_2+\mid\mid\theta\mid\mid_2}$  
（当$\varepsilon=10^{-7}$，上值为$10^{-7}$就还不错，如果是$10^{-3}$就可能有问题了）  

notes
- only debug，not train
- remember regularization
- Does not work with dropout
- Rarely, dw is correct when w is close to 0, so train for a while before do gradient check.  
