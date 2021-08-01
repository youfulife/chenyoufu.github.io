---
title: "1319 连通网络的操作次数"
date: 2021-08-01T15:02:38+08:00
draft: false
tags: ["算法", "并查集"]
categories: ["技术"]
---

**题目**

用以太网线缆将 n 台计算机连接成一个网络，计算机的编号从 0 到 n-1。线缆用 connections 表示，其中 connections[i] = [a, b] 连接了计算机 a 和 b。

网络中的任何一台计算机都可以通过网络直接或者间接访问同一个网络中其他任意一台计算机。

给你这个计算机网络的初始布线 connections，你可以拔开任意两台直连计算机之间的线缆，并用它连接一对未直连的计算机。请你计算并返回使所有计算机都连通所需的最少操作次数。如果不可能，则返回 -1 。 

```
输入：n = 4, connections = [[0,1],[0,2],[1,2]]
输出：1
解释：拔下计算机 1 和 2 之间的线缆，并将它插到计算机 1 和 3 上。
```
链接：https://leetcode-cn.com/problems/number-of-operations-to-make-network-connected

**解题思路**

**并查集**


```python
class Solution(object):

    class UFS(object):

        def __init__(self, n):
            self.n = n
            self.parent = [i for i in range(n)]

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
        
        def size(self):
            cnt = 0
            for i in range(self.n):
                if i == self.parent[i]:
                    cnt += 1
            return cnt

    def makeConnected(self, n, connections):
        ufs = Solution.UFS(n)
        extra = []
        for c in connections:
            if not ufs.isConnected(c[0], c[1]):
                ufs.union(c[0], c[1])
            else:
                extra.append(c)
        
        need = ufs.size() - 1
        if need > len(extra):
            return -1
        
        return need
```