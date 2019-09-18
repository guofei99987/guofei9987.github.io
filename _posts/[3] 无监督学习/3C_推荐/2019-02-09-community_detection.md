---
layout: post
title: 【图挖掘】社区检测
categories:
tags: 3_3_推荐
keywords:
description:
order: 350
---

## 用途
- 识别有影响力的人或信息
- 发现子图模式、社区发现
- 图分类（广告或产品的目标人群定位）
- 发现时间序列模式

## 1. 聚类算法

step1. 由网络结构计算距离矩阵；  
step2. 用距离确定节点相似度；  
step3. 根据相似度从强到弱递归合并节点；  
step4. 根据实际需求横切树状图 ；  

树状图类似这个：
![](http://www.guofei.site/public/postimg/hierachicalcluster.png)

## 图划分
从一个整体切分成两个，算法目标：
1. 切分后的两个子图规模相近
2. 被切分的边数量最少（或者用权重和作为指标）

*明尼苏达大学的METIS是最权威的图划分工具*

## 分裂算法（GN算法）

**思路** ：定义边介数（betweenness）指标（衡量的是网络里一个边占据其它节点间捷径的程度），具有高边介数的边代表了社区的边界；  

**算法**：
step1 找到网络中具有最大边介数的边；  
step2 删除该边；  
step3 重复 step1 和 step2，直到所有边被移除；  

边介数最常见的定义：图中通过该边的所有最短路径数量  

![](https://github.com/guofei9987/pictures_for_blog/blob/master/graph/gn_algorithm.png?raw=true)


## 模块度优化算法

**思路** ：
1. 定义模块度（Modularity）指标，用来衡量一个社区的划分好坏
2. 以模块度为目标进行优化。例如在层次化聚类中使用贪婪算法


一种模块度的定义：  
Q=社区内的边占比 – 社区的边占比平方  

具体[看这里](https://blog.csdn.net/marywbrown/article/details/62059231)

## 标签传播算法（LPA）

这是一种启发式算法，**思路** 是，一个节点应该与多数邻居在同一社区内。  

**算法**：  
step1：给每个节点初始化一个标签；  
step2：在网络中传播标签；  
step3：选择邻居的标签中数量最多的进行更新；  
step4：重复步骤2和3，直到收敛或满足迭代次数；  


特点：
1）适合半监督和无监督；  
2）效率很高，适合大规模；  
3）存在振荡（因为采用异步更新）；  
4）结果可能不稳定；   


找到两个实现
```py
nx.algorithms.community.label_propagation.asyn_lpa_communities(G)
nx.algorithms.community.label_propagation.label_propagation_communities(G)

# 暂时不知道啥区别，可能是这样的：
# 1是异步，2是半同步
# 1返回<dict_valueiterator>，2返回<generator object label_propagation_communities>
# 1结果不稳定，2结果稳定
# 1分的类较少，2分的类较多
# 1可以用于有向图，2不能
```

## 随机游走

**思路** 启发式规则：  
1） 从节点出发随机游走，停留在社区内的概率高于到达社区外的；  
2）重复随机游走，强化并逐渐显现社区结构。  


**算法**
1）建立邻接矩阵（含自环）；  
2）标准化转移概率矩阵；  
3）Expansion操作，对矩阵计算e次幂方 ；  
4）Inflation操作，对矩阵元素计算r次幂方并标准化（强化紧密的点，弱化松散的点 ）  
5）重复4和5直到稳定；  
6）对结果矩阵进行常规聚类；  


## 其它算法
- 派系过滤算法（clique percolation algorithm）- 社区的网络
- 领导力扩张（Leadership expansion）- 类似与kmeans
- 基于聚类系数的方法（Maximal K-Mutual friends）- 目标函数优化
- HANP（Hop attenuation & node preference)- LPA增加节点传播能力
- SLPA（Speak-Listen Propagation Algorithm)- 记录历史标签序列
- Matrix blocking – 根据邻接矩阵列向量的相似性排序
- Skeleton clustering – 映射网络到核心连接树后进行检测
