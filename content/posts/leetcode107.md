---
title: "107 二叉树的层序遍历 II"
date: 2021-08-09T23:01:30+08:00
draft: false
tags: ["算法", "树", "BFS"]
categories: ["技术"]
---

**题目**

给定一个二叉树，返回其节点值自底向上的层序遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

例如：
```
给定二叉树 [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其自底向上的层序遍历为：

[
  [15,7],
  [9,20],
  [3]
]
```

链接：https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii

**解题思路**

BFS 标准模版，背诵默写即可。

```python
class Solution(object):
    def levelOrderBottom(self, root):
        if not root:
            return []
        
        ans = []
        q = deque([root])
        while q:
            level = []
            for _ in range(len(q)):
                node = q.popleft()
                level.append(node.val)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            ans.append(level)
        return ans[::-1]
```
