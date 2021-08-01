---
title: "684 冗余连接"
date: 2021-08-01T10:42:12+08:00
draft: false
tags: ["算法", "并查集"]
categories: ["技术"]
---

**题目**

树可以看成是一个连通且 无环 的 无向 图。

给定往一棵 n 个节点 (节点值 1～n) 的树中添加一条边后的图。添加的边的两个顶点包含在 1 到 n 中间，且这条附加的边不属于树中已存在的边。图的信息记录于长度为 n 的二维数组 edges ，edges[i] = [ai, bi] 表示图中在 ai 和 bi 之间存在一条边。

请找出一条可以删去的边，删除后可使得剩余部分是一个有着 n 个节点的树。如果有多个答案，则返回数组 edges 中最后出现的边。

链接：https://leetcode-cn.com/problems/redundant-connection

**解题思路**

并查集

```python
class Solution(object):

    class UFS(object):
        def __init__(self, n):
            self.parent = [i for i in range(n)]
            self.n = n

        def find(self, p):
            if p == self.parent[p]:
                return p
            self.parent[p] = self.find(self.parent[p])
            return self.parent[p]
        
        def union(self, p, q):
            self.parent[self.find(p)] = self.find(q)
            return
        
        def isConnected(self, p, q):
            return self.find(p) == self.find(q)


    def findRedundantConnection(self, edges):
        n = len(edges)
        ufs = Solution.UFS(n)
        for edge in edges:
            # 这里要注意，并查集的下标从0开始，题目给出的节点是从1开始
            p = edge[0] - 1
            q = edge[1] - 1
            if not ufs.isConnected(p, q):
                ufs.union(p, q)
            else:
                return edge
        return
```