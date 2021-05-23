---
title: "404 左叶子之和"
date: 2021-05-23T22:11:47+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

**题目**

计算给定二叉树的所有左叶子之和。

**示例：**
```
    3
   / \
  9  20
    /  \
   15   7
```
在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24

链接：https://leetcode-cn.com/problems/sum-of-left-leaves

**解题思路**

二叉树遍历，加一个标记来区分当前节点是否是左节点。

```python
class Solution(object):
    def sumOfLeftLeaves(self, root):
        if not root:
            return 0
        def helper(root, sum, isLeft):
            if not root.left and not root.right and isLeft:
                sum.append(root.val)
                return
            
            if root.left:
                helper(root.left, sum, True)
            
            if root.right:
                helper(root.right, sum, False)
        s = []
        helper(root, s, False)
        return sum(s)
```

非递归

```python
class Solution(object):
    def sumOfLeftLeaves(self, root):
        if not root:
            return 0
        
        ans = 0
        stack = [(root, False)]
        while stack:
            node, isLeft = stack.pop()
            if not node.left and not node.right and isLeft:
                ans += node.val
            if node.right:
                stack.append((node.right, False))
            if node.left:
                stack.append((node.left, True))
        return ans
```