---
title: "104 二叉树的最大深度"
date: 2021-04-19T22:42:06+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

说明: 叶子节点是指没有子节点的节点。

示例：
给定二叉树 [3,9,20,null,null,15,7]，

```
    3
   / \
  9  20
    /  \
   15   7
```
返回它的最大深度 3 。

链接：https://leetcode-cn.com/problems/maximum-depth-of-binary-tree

**解题思路**

递归，深度优先，发现叶子结点就判断当前最大深度。

自己写的第一遍直接把maxdepth作为参数传进去了，后来发现整数不能这么干。于是声明为一个类变量，相当于全局变量，引用的时候要通过self.maxdepth的形式来用。

```python
class Solution(object):
    maxdepth = 0
    def maxDepth(self, root):
        if not root:
            return 0
        self.helper(root, 1)
        return self.maxdepth
    
    def helper(self, root, height):

        if not root.left and not root.right:
            self.maxdepth = max(height, self.maxdepth)

        if root.left:
            self.helper(root.left, height+1)

        if root.right:
            self.helper(root.right, height+1) 
```

官方题解的算法，更加简洁，而且不需要辅助函数。

if the node does not exist, simply return 0. Otherwise, return the 1+the longer distance of its subtree.

```python
class Solution(object):
    def maxDepth(self, root):
        if not root:
            return 0
        return max(self.maxDepth(root.left), self.maxDepth(root.right)) + 1
```

广度优先, 还是一层一层的来，跟之前层级相关题目的区别是，不需要去保存每一层节点的值。

```python
class Solution(object):
    def maxDepth(self, root):
        if not root:
            return 0
        
        q = deque([root])
        ans = 0
        while q:
            for _ in range(len(q)):
                node = q.popleft()
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            ans += 1
        return ans
```