---
title: "778 水位上升的泳池中游泳"
date: 2021-08-05T23:12:37+08:00
draft: false
tags: ["算法", "并查集", "五星", "二分法", "Dijstra", "Flood Fill"]
categories: ["技术"]
---

**题目**

在一个 N x N 的坐标方格 grid 中，每一个方格的值 grid[i][j] 表示在位置 (i,j) 的平台高度。

现在开始下雨了。当时间为 t 时，此时雨水导致水池中任意位置的水位为 t 。你可以从一个平台游向四周相邻的任意一个平台，但是前提是此时水位必须同时淹没这两个平台。假定你可以瞬间移动无限距离，也就是默认在方格内部游动是不耗时的。当然，在你游泳的时候你必须待在坐标方格里面。

你从坐标方格的左上平台 (0，0) 出发。最少耗时多久你才能到达坐标方格的右下平台 (N-1, N-1)？

链接：https://leetcode-cn.com/problems/swim-in-rising-water

**解题思路**

并查集

由于题目要我们找的是最少等待时间，可以模拟下雨的过程，把网格抽象成一个无权图，每经过一个时刻，就考虑此时和雨水高度相等的单元格，考虑这个单元格的上、下、左、右、四个方向，并且高度更低的单元格，把它们在并查集中进行合并。

从最小高度开始，依次合并它的相邻节点（上下左右）中高度小于它的单元格，直到左上角到右下角联通为止。

```python
class Solution(object):

    class UFS(object):
        def __init__(self, n):
            self.count = n
            self.p = [i for i in range(n)]

        def find(self, x):
            if x != self.p[x]:
                self.p[x] = self.find(self.p[x])
            return self.p[x]
        
        def union(self, x, y):
            x_root = self.find(x)
            y_root = self.find(y)
            if x_root == y_root:
                return 
            self.p[x_root] = y_root
            self.count -= 1
            return
        
        def isConnected(self, x, y):
            return self.find(x) == self.find(y)

    def swimInWater(self, grid):
        n = len(grid)
        ufs = Solution.UFS(n * n)

        h = {}
        for i in range(n):
            for j in range(n):
                x = grid[i][j]
                h[x] = (i, j)
        t = 0
        while t < n * n:
            i, j = h[t]
            if i > 0 and grid[i-1][j] < t:
                ufs.union(i * n + j, (i - 1) * n + j)
            if i < n-1  and grid[i+1][j] < t:
                ufs.union(i * n + j, (i + 1) * n + j)
            if j > 0  and grid[i][j-1] < t:
                ufs.union(i * n + j, i * n + j - 1)
            if j < n-1 and grid[i][j+1] < t:
                ufs.union(i * n + j, i * n + j + 1)
            
            if ufs.isConnected(0, n*n-1):
                break
            t += 1
        return t
```

**二分法**

**Dijstra**