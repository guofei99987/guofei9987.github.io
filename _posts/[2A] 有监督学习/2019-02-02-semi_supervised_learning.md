---
layout: post
title: 【半监督学习】理论篇
categories:
tags: 2A_有监督学习
keywords: model evaluation
description:
order: 280
---

## 介绍
有时候，我们的数据是这样的：l个样本是 labeled 的，u个样本是 unlabeled，并且$l\ll u$，我们希望把u个样本也用起来。  

- **主动学习（active learning）**：先在$D_l$上训练一个模型，然后用这个模型挑选$D_u$上一个队模型改善最有帮助的样本，获取其label。这种方法优点是大幅降低标记成本。缺点是引入专家，通过外部交互来获取额外信息。
- **半监督学习（semi-supervised learning）**：不依赖外界交互，但也是用 unlabeled sample 的方法。现实中往往获取标签是昂贵的，而获取数据是廉价的，如医学影像等。


半监督学习又分两类：
- **pure semi-supervised learning** 假定 unlabeled sample 不是待预测的样本。
- **transductive learning（直推学习）** 假定 unlabeled sample 是待预测的样本


## 生成式模型
假设所有数据都是由同一个潜在模型“生成”的

## 半监督SVM
**Semi-Supervised Support Vector Machine（半监督支持向量机）** 思路是找到这样的超平面：
1. 能将两类 labeled sample 分开
2. 穿过数据低密度区域，**“低密度分割”（low-density separation）**


### 模型
最著名的是 **TSVM（Transductive Support Vector Machine）** (针对二分类的半监督SVM方法)  
思路是对所有的 unlabeled sample 进行各种可能的标记指派（label assignment，也就是尝试对每个 unlabeled sample 作为正例或反例），然后在所有这些结果中找到间隔最大化的超平面。  

数学表示：  
$$D_l=\{ (x_1,y_1),(x_2,y_2),...()\}$$
$\min\limits_{w,b,\hat y,\xi} \dfrac{1}{2}\mid\mid w\mid\mid_2^2 + C_l\sum\limits_{i \in labeled}\xi_i+C_u\sum\limits_{i \in unlabeled}\xi_i$  
s.t.$y_i(w^Tx_i+b)\geq 1-\xi_i,i \in labeled$  
$\hat y_i(w^Tx_i+b)\geq1-\xi_i,i \in unlabeled$  
$\xi_i\geq,i\in labeled \cup unlabeled$  


### 算法
直接解上面这个最优化有些困难，用迭代方法来解。


## 参考资料

周志华：《机器学习》
