---
layout: post
title: 【Python数据结构5】Binary Search Tree
categories:
tags: 5数据结构与算法
keywords:
description:
order: 555
---
`Binary Search Tree`(BST) is a special form of a binary tree.  
- The value in each node must be greater than (or equal to) any values in its left subtree
- but less than (or equal to) any values in its right subtree


```py
class Solution:
    def inorder(self, root):  # LDR
        return [] if (root is None) else self.inorder(root.left) + [root.val] + self.inorder(root.right)

    def preorder(self, root):  # DLR
        return [] if (root is None) else [root.val] + self.preorder(root.left) + self.preorder(root.right)

    def postorder(self, root):  # LRD
        return [] if (root is None) else self.inorder(root.left) + self.inorder(root.right) + [root.val]

    def isValidBST(self,root):
        # 判断一个二叉树是否是 BST
        inorder=self.inorder(root)
        return inorder==list(sorted(set(inorder)))
```
