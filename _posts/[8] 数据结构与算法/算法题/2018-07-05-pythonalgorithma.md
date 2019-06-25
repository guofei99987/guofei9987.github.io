---
layout: post
title: 【Python】【算法题集3】
categories:
tags: 8_数据结构与算法
keywords:
description:
order: 591
---

- 入门级题目：[【Python】【算法题集1】](http://www.guofei.site/2017/05/03/TrickPython.html)
- 《编程之美》中的题目：[【Python】【算法题集2】](http://www.guofei.site/2017/08/28/someproblems.html)
- LeetCode上的题目：[【Python】【算法题集3】](http://www.guofei.site/2018/07/05/pythonalgorithma.html)


这一部分是我刷LeetCode的感想（限于数据结构和算法部分），刷完的题目见于这个[GitHub库](https://github.com/guofei9987/leetcode_python)


## 287. Find the Duplicate Number
https://leetcode.com/problems/find-the-duplicate-number/solution/  
有两个思想
1. 1-n这n个数组成一个list，必然可以遍历
2. fast，slow 两个指针必然可以做到任意两个元素一一对应

## 658. Find K Closest Elements
https://leetcode.com/explore/learn/card/binary-search/135/template-iii/945/

使用Python的 sorted ，其time complexity 是O(log(n))


## 160. Intersection of Two Linked Lists
把 two pointer technique 用到极致了
https://leetcode.com/explore/learn/card/linked-list/214/two-pointer-technique/1215/discuss/49798/Concise-python-code-with-comments/156662  

另一个巧妙使用 two pointer technique 的题目 ([142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/description/))
