---
layout: post
title: 【DeepDream】初学
categories:
tags: 2B神经网络与TF
keywords:
description:
order: 262
---

[tensorflow 示例](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/examples/tutorials/deepdream) 上的讲解，比看过的几本书都要清晰（看的几本书都是从这里抄的，而且抄来抄去反而不太好看了）。  


我的粗浅理解是这样的：
1. 找到CNN中某个神经元（换句话说，某个CNN节点的某个channel）。合适的层叫做风格层，至于怎么找到的我还没搞清楚。总之越往上层的神经元，其处理的信息越抽象；越下层的神经元越细节。
2. 对于输入的图片，我们想调整图片，使得上面那个神经元输出值最大（被激活）
3. 用梯度来做这个事情，迭代中不断调整图片各个像素的值（梯度上升）。






## 参考文献
https://github.com/tensorflow/tensorflow/tree/master/tensorflow/examples/tutorials/deepdream  
https://github.com/hjptriplebee/deep_dream_tensorflow/blob/master/main.py  
https://github.com/google/deepdream/blob/master/dream.ipynb
