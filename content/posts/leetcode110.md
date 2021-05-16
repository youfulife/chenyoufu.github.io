---
title: "110 判断是否是平衡二叉树"
date: 2021-05-14T22:49:49+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---


**题目**

给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

一个二叉树每个节点 的左右两个子树的高度差的绝对值不超过 1 。


**解题思路**

自顶向下，根据定义，暴力循环，计算每一个节点的左右子树的高度差。

时间复杂度O(N^2)。

```python
class Solution(object):
    def isBalanced(self, root):
        if not root:
            return True
        if abs(self.height(root.left) - self.height(root.right)) > 1:
            return False
        return self.isBalanced(root.left) and self.isBalanced(root.right)
    
    def height(self, root):
        if not root:
            return 0
        return 1 + max(self.height(root.left), self.height(root.right))
```

**优化 自底向上**

方法一由于是自顶向下递归，因此对于同一个节点，函数 \texttt{height}height 会被重复调用，导致时间复杂度较高。如果使用自底向上的做法，则对于每个节点，函数 \texttt{height}height 只会被调用一次。

自底向上递归的做法类似于后序遍历，对于当前遍历到的节点，先递归地判断其左右子树是否平衡，再判断以当前节点为根的子树是否平衡。如果一棵子树是平衡的，则返回其高度（高度一定是非负整数），否则返回 -1−1。如果存在一棵子树不平衡，则整个二叉树一定不平衡。



```python
class Solution(object):
    def isBalanced(self, root):
        return self.height(root) >= 0

    def height(self, root):
        if not root:
            return 0
        leftH = self.height(root.left)
        rightH = self.height(root.right)
        if leftH == -1 or rightH == -1 or abs(leftH-rightH) > 1:
            return -1
      
        return max(leftH, rightH) + 1
```
