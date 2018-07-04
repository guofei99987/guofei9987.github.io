---
layout: post
title: 【Python数据结构】线性表、堆栈、队列
categories:
tags: 5数据机构与算法
keywords:
description:
order: 551
---

目前还没有找到 Python 方面关于数据结构的很系统的教材，这部分内容是我参考C语言数据结构教材，部分灵感来自 LeetCode （附上本人的 [LeetCode解答](https://github.com/guofei9987/leetcode_python)  
有一些数据结构在Python中无需实现，但在编程中往往借助其思路。例如，使用list模拟堆栈和队列，这种情况也归于相应的部分。  

## 1. 线性表
线性表包括
- 顺序表
- 单链表
- 单循环链表
- 双向循环链表



### 1.1 顺序表
顺序表是在内存中连续存放的数组。  
顺序表有如下操作：
- 初始化
- 求元素个数
- 在i位置插入一个元素。从后往前，依次后移1格，直到i位置。
- 删除一个元素。类似的相反操作


因此，顺序表的增、删操作，时间复杂度都是 O(n)


### 1.2 单链表
有两种：带头结点单链表（用一个空节点作为头部结点），不带头结点单链表（用第一个数据节点作为头部结点）  
不带头结点单链表对第一个元素增、删时，与其它元素的增删操作不一致，所以一般使用带头结点  







## list实现栈和队列
我们知道，list天然地适合做 Stack，即尾部入，尾部出。  
我们又知道 list 删除头部的元素是极为低效的，解决方法是很简单，只要增加一个指向头部的指针即可，。  

### 堆栈（先进先出表）


### 队列
题目灵感来自[LeetCode题目](https://leetcode.com/problems/baseball-game/discuss/119575/Python-4-liner),我给出的解答见于[这里](https://github.com/guofei9987/leetcode_python/blob/master/%5B682%5D%5BBaseball%20Game%5D%5BEasy%5D.py)  

例子：
```python
input_queue=[1,2,3,4,5]
stack=[2,1]
pointer=0
for i in input_queue:
    stack.append(i)
    # 后两行是先出的功能：
    stack_out=stack[pointer]
    pointer+=1

# 事实上，因为可以使用stack[-1],stack[-2]这些命令，所以 pointer 这个变量往往不必定义
# 会有内存浪费，定期清理即可，参考代码： stack=stack[pointer:]
```
