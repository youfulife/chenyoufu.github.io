---
title: "129 求根节点到叶节点数字之和"
date: 2021-09-04T13:20:05+08:00
draft: false
tags: ["算法", "树", "DFS", "BFS", "五星"]
categories: ["技术"]
---

**题目**

给你一个二叉树的根节点 root ，树中每个节点都存放有一个 0 到 9 之间的数字。
每条从根节点到叶节点的路径都代表一个数字：

例如，从根节点到叶节点的路径 1 -> 2 -> 3 表示数字 123 。
计算从根节点到叶节点生成的 所有数字之和 。

叶节点 是指没有子节点的节点。

链接：https://leetcode-cn.com/problems/sum-root-to-leaf-numbers

**解题思路**

DFS

```python
class Solution(object):
    def sumNumbers(self, root):
        if not root:
            return 0

        def dfs(root, k, ans):

            if not root.left and not root.right:
                ans.append(k)
                return
            
            if root.left:
                dfs(root.left, k + str(root.left.val), ans)
            if root.right:
                dfs(root.right, k + str(root.right.val), ans)

        ans = []
        dfs(root, '' + str(root.val), ans)
        return sum([int(x) for x in ans])
```