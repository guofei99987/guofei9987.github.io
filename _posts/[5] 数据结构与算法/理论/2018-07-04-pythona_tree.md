---
layout: post
title: 【Python数据结构4】Tree
categories:
tags: 5数据结构与算法
keywords:
description:
order: 554
---
## 定义二叉树

这里除了定义二叉树外，还实现了以下功能：
- 打印二叉树
- 前序遍历
- 中序遍历
- 后续遍历
- 查找路径


```py
# Definition for a binary tree node.
class TreeNode(object):
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

    def printtree(self, i=0):
        '''
        :param i:
        :return:
        打印二叉树，凹入表示法。原理是RDL遍历，旋转90度看
        '''
        tree_str=''
        if self.right:
            tree_str+=self.right.printtree(i + 1)
        if self.val:
            tree_str+=('    ' * i + '-' * 3+str(self.val)+'\n')
        if self.left:
            tree_str+=self.left.printtree(i + 1)
        return tree_str

    def PreOrder(self, list_num=[]):  # DLR
        list_num.append(self.val)
        if self.left is not None:
            self.left.PreOrder(list_num=list_num)
        if self.right is not None:
            self.right.PreOrder(list_num=list_num)
        return list_num

    def InOrder(self, list_num=[]):  # LDR
        if self.left is not None:
            self.left.InOrder(list_num=list_num)
        list_num.append(self.val)
        if self.right is not None:
            self.right.InOrder(list_num=list_num)
        return list_num

    def PostOrder(self, list_num=[]):  # LRD
        if self.left is not None:
            self.left.PostOrder(list_num=list_num)
        if self.right is not None:
            self.right.PostOrder(list_num=list_num)
        list_num.append(self.val)
        return list_num

    def find_track(self, num, track_str=''):
        track_str = track_str + str(self.val)
        if self.val == num:
            print(track_str)
        if self.left is not None:
            self.left.find_track(num, track_str + ' ->left-> ')
        if self.right is not None:
            self.right.find_track(num, track_str + ' ->right-> ')
```

把方法封装到 TreeNode 外
```py
class TreeNode(object):
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None

class Solution:
    def inorder(self, root):  # LDR
        return [] if (root is None) else self.inorder(root.left) + [root.val] + self.inorder(root.right)

    def preorder(self, root):  # DLR
        return [] if (root is None) else [root.val] + self.preorder(root.left) + self.preorder(root.right)

    def postorder(self, root):  # LRD
        return [] if (root is None) else self.inorder(root.left) + self.inorder(root.right) + [root.val]

    def isValidBST(self,root):
        inorder=self.inorder(root)
        return inorder==list(sorted(set(inorder)))
```

## 常用函数
- 顺序存储转链式存储
- 链式存储转顺序存储（尚未实现）


```py
# 顺序结构转链式结构
# 第i个节点（编号从1开始）
def list2tree(i=1, list_num=[1, 2, 3]):
    if i > len(list_num):
        return None
    treenode = TreeNode(list_num[i - 1])
    treenode.left = list2tree(2 * i, list_num)
    treenode.right = list2tree(2 * i + 1, list_num)
    return treenode


def tree2list():
    pass


list_num = [1, 2, 3, 4, 5, 6, 7]
tree = list2tree(1, list_num)
# tree.printtree()
tree.PreOrder()
tree.InOrder()
tree.PostOrder()
tree.find_track(4)
```

### 另一种实现
这个来自[leetCode](https://leetcode.com/problems/recover-binary-search-tree/discuss/32539/Tree-Deserializer-and-Visualizer-for-Python)，做了一定的改造  
```py
def list2tree2(nums):
  # 与我写的 list2tree 的区别是：对空值不再生成子节点，之后的数据也不会作为这个空节点的子节点，而是跳过，因此更加节省空间。
    if not nums:
        return None
    nodes = [None if val is None else TreeNode(val)
             for val in nums]
    kids = nodes[::-1]
    root = kids.pop()
    for node in nodes:
        if node:
            if kids: node.left = kids.pop()
            if kids: node.right = kids.pop()
    return root


# LeetCode官方版本
# deserialize('[2,1,3,0,7,9,1,2,null,1,0,null,null,8,8,null,null,null,null,7]')
def deserialize(string):
    if string == '{}':
        return None
    nodes = [None if val == 'null' else TreeNode(int(val))
             for val in string.strip('[]{}').split(',')]
    kids = nodes[::-1]
    root = kids.pop()
    for node in nodes:
        if node:
            if kids: node.left  = kids.pop()
            if kids: node.right = kids.pop()
    return root


def drawtree(root):
    # 用 turtle 画 Tree，比纯字符串美观，但慢
    def height(root):
        return 1 + max(height(root.left), height(root.right)) if root else -1

    def jumpto(x, y):
        t.penup()
        t.goto(x, y)
        t.pendown()

    def draw(node, x, y, dx):
        if node:
            t.goto(x, y)
            jumpto(x, y - 20)
            t.write(node.val, align='center', font=('Arial', 12, 'normal'))
            draw(node.left, x - dx, y - 60, dx / 2)
            jumpto(x, y - 20)
            draw(node.right, x + dx, y - 60, dx / 2)
    import turtle
    t = turtle.Turtle()
    t.speed(0);
    turtle.delay(0)
    h = height(root)
    jumpto(0, 30 * h)
    draw(root, 0, 30 * h, 40 * h)
    t.hideturtle()
    turtle.mainloop()

drawtree(list2tree2([2, 1, 3, 0, 7, 9, 1, 2, None, 1, 0, None, None, 8, 8, None, None, None, None, 7]))
```


## 其它应用举例

### Level Order Traversal
[效果描述](https://leetcode.com/problems/binary-tree-level-order-traversal/description/)  
代码本身应用不广，这个思路可以用来做 list2tree 的逆问题（想想怎样让程序更简单）
```py
class Solution:
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if root is None:
            return []
        import collections
        level = 0
        deque = collections.deque([(root,level)])
        output=[]
        while deque:
            tmp_root,level = deque.popleft()
            if tmp_root.left: deque.append((tmp_root.left,level+1))
            if tmp_root.right: deque.append((tmp_root.right,level+1))
            if len(output)<=level:
                output.append([tmp_root.val])
            else:
                output[level].append(tmp_root.val)
        return output
```

### Recursive
"Top-down" Solution
```py
1. return specific value for null node
2. update the answer if needed                      // anwer <-- params
3. left_ans = top_down(root.left, left_params)      // left_params <-- root.val, params
4. right_ans = top_down(root.right, right_params)   // right_params <-- root.val, params
5. return the answer if needed                      // answer <-- left_ans, right_ans
```
"Bottom-up" Solution
```py
1. return specific value for null node
2. left_ans = bottom_up(root.left)          // call function recursively for left child
3. right_ans = bottom_up(root.right)        // call function recursively for right child
4. return answers                           // answer <-- left_ans, right_ans, root.val
```

对于 “max-depth” 问题，具体解法如下：
```py
1. return if root is null
2. if root is a leaf node:
3.      answer = max(answer, depth)         // update the answer if needed
4. maximum_depth(root.left, depth + 1)      // call the function recursively for left child
5. maximum_depth(root.right, depth + 1)     // call the function recursively for right child

1. return 0 if root is null                 // return 0 for null node
2. left_depth = maximum_depth(root.left)
3. right_depth = maximum_depth(root.right)
4. return max(left_depth, right_depth) + 1  // return depth of the subtree rooted at root


class Solution:
    def maxDepth(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """
        return 0 if root is None else max(self.maxDepth(root.left),self.maxDepth(root.right))+1
```



### mergeTrees
```py
# merge
# Given two binary trees and imagine that when you put one of them to cover the other,
# some nodes of the two trees are overlapped while the others are not.
# merge them into a new binary tree.
# The merge rule is that if two nodes overlap, then sum node values up as the new value of the merged node.
# Otherwise, the NOT null node will be used as the node of new tree.
#
# Example :
# Input:
# 	Tree 1                     Tree 2
#           1                         2
#          / \                       / \
#         3   2                     1   3
#        /                           \   \
#       5                             4   7
# Output:
# Merged tree:
# 	     3
# 	    / \
# 	   4   5
# 	  / \   \
# 	 5   4   7

def mergeTrees(t1, t2):
    """
    :type t1: TreeNode
    :type t2: TreeNode
    :rtype: TreeNode
    """
    if (t1.val is not None) and (t2.val is not None):
        root = TreeNode(t1.val + t2.val)
        root.left = mergeTrees(t1.left, t2.left)
        root.right = mergeTrees(t1.right, t2.right)
        return root
    else:
        return t1 or t2

t1=list2tree(i=1,list_num=[1,2,3,4,None,None,5])
t2=list2tree(i=1,list_num=[2,9,None,None,3,None,None])

a=mergeTrees(t1, t2)
```


## 其它
```py
# 先子节点，后兄弟节点”的表示法
class Tree:
    def __init__(self, kids, next=None):
        self.kids = self.val = kids
        self.next = next
```
