---
title: "547 省份数量"
date: 2021-08-01T10:00:46+08:00
draft: false
tags: ["算法", "并查集", "BFS", "DFS"]
categories: ["技术"]
---
**题目**
有 n 个城市，其中一些彼此相连，另一些没有相连。如果城市 a 与城市 b 直接相连，且城市 b 与城市 c 直接相连，那么城市 a 与城市 c 间接相连。

省份 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 n x n 的矩阵 isConnected ，其中 isConnected[i][j] = 1 表示第 i 个城市和第 j 个城市直接相连，而 isConnected[i][j] = 0 表示二者不直接相连。

返回矩阵中 省份 的数量。

链接：https://leetcode-cn.com/problems/number-of-provinces

**解题思路**

**并查集**

```python
class Solution(object):

    class UFS():
        def __init__(self, n):
            self.n = n
            self.parent = [i for i in range(n)]

        def find(self, p):
            if p != self.parent[p]:
                self.parent[p] = self.find(self.parent[p])
            return self.parent[p]
        
        def union(self, p, q):
            self.parent[self.find(p)] = self.find(q)
        
        def setCount(self):
            cnt = 0
            for i in range(self.n):
                if self.parent[i] == i:
                    cnt += 1
            return cnt

    def findCircleNum(self, isConnected):
        n = len(isConnected)
        ufs = Solution.UFS(n)
        for i in range(n):
            for j in range(n):
                if isConnected[i][j] == 1:
                    ufs.union(i, j)
        return ufs.setCount()
```

**DFS**

**BFS**


-----------

2021.08.31 二刷 一遍过 标准并查集模版

```python
class Solution(object):
    class UFS():

        def __init__(self, n):
            self.count = n
            self.p = [i for i in range(n)]
        
        def find(self, x):
            if self.p[x] != x:
                self.p[x] = self.find(self.p[x])
            return self.p[x]
        
        def union(self, x, y):
            rootx = self.find(x)
            rooty = self.find(y)
            if rootx == rooty:
                return
            self.p[rootx] = rooty
            self.count -= 1
            return
        
        def isConnected(self, x, y):
            return self.find(x) == self.find(y)



    def findCircleNum(self, isConnected):
        """
        :type isConnected: List[List[int]]
        :rtype: int
        """
        n = len(isConnected)

        ufs = Solution.UFS(n)
        for i in range(n):
            for j in range(n):
                if isConnected[i][j] == 1:
                    ufs.union(i, j)
        return ufs.count
```