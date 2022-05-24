---
title: "图系列之并查集"
date: 2022-04-27T22:38:22+08:00
draft: false
tags: ["算法", "并查集", "五星"]
categories: ["技术"]
---

### 经典题目

* [547. 省份数量](https://leetcode-cn.com/problems/number-of-provinces)



### 547. 省份数量

有 n 个城市，其中一些彼此相连，另一些没有相连。如果城市 a 与城市 b 直接相连，且城市 b 与城市 c 直接相连，那么城市 a 与城市 c 间接相连。

省份 是一组直接或间接相连的城市，组内不含其他没有相连的城市。

给你一个 n x n 的矩阵 isConnected ，其中 isConnected[i][j] = 1 表示第 i 个城市和第 j 个城市直接相连，而 isConnected[i][j] = 0 表示二者不直接相连。

返回矩阵中 省份 的数量。

链接：https://leetcode-cn.com/problems/number-of-provinces

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