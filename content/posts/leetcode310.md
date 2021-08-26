---
title: "310 最小高度树"
date: 2021-08-26T21:58:43+08:00
draft: false
tags: ["算法", "图", "拓扑排序", "BFS", "逆向思维", "五星"]
categories: ["技术"]
---

**题目**

给你一棵包含 n 个节点的树，标记为 0 到 n - 1 。给定数字 n 和一个有 n - 1 条无向边的 edges 列表（每一个边都是一对标签），其中 edges[i] = [ai, bi] 表示树中节点 ai 和 bi 之间存在一条无向边。

可选择树中任何一个节点作为根。当选择节点 x 作为根节点时，设结果树的高度为 h 。在所有可能的树中，具有最小高度的树（即，min(h)）被称为 最小高度树 。

请你找到所有的 最小高度树 并按 任意顺序 返回它们的根节点标签列表。

链接：https://leetcode-cn.com/problems/minimum-height-trees

**解题思路**

关键是找到bfs的起点，然后每次缩小范围。

一圈一圈的剪除最外层的叶子结点，最后剩下的两个或一个节点就是根节点。

```python
class Solution(object):
    def findMinHeightTrees(self, n, edges):
        if n == 1:
            return [0]

        v = [[] for _ in range(n)]

        for x, y in edges:
            v[x].append(y)
            v[y].append(x)
        
        q = deque([i for i in range(n) if len(v[i]) == 1])
        ans = []
        while q:
            leaves = []
            for _ in range(len(q)):
                i = q.popleft()
                leaves.append(i)
                for j in v[i]:
                    v[j].remove(i)
                    if len(v[j]) == 1:
                        q.append(j)
            ans = leaves
        return ans
```