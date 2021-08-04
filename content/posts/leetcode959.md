---
title: "959 由斜杠划分区域"
date: 2021-08-04T23:24:04+08:00
draft: false
tags: ["算法", "并查集", "五星", "DFS", "BFS"]
categories: ["技术"]
---
**题目**
在由 1 x 1 方格组成的 N x N 网格 grid 中，每个 1 x 1 方块由 /、\ 或空格构成。这些字符会将方块划分为一些共边的区域。

（请注意，反斜杠字符是转义的，因此 \ 用 "\\" 表示。）。

返回区域的数目。

链接：https://leetcode-cn.com/problems/regions-cut-by-slashes

**解题思路**

直接看官方题解

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
            
            self.p[x_root] = self.p[y_root]
            self.count -= 1
            return

    def regionsBySlashes(self, grid):
        n = len(grid)
        ufs = Solution.UFS(4 * n * n)
        for i in range(n):
            for j in range(n):
                index = 4 * (i * n + j)
                c = grid[i][j]
                if c == ' ':
                    ufs.union(index, index+1)
                    ufs.union(index+1, index+2)
                    ufs.union(index+2, index+3)
                elif c == '/':
                    ufs.union(index, index + 3)
                    ufs.union(index + 1, index + 2)
                else:
                    ufs.union(index, index + 1)
                    ufs.union(index + 2, index + 3)
                
                if j < n - 1:
                    ufs.union(index + 1, 4 * (i * n + j + 1) +3)
                if i < n - 1:
                    ufs.union(index + 2,  4 * ((i + 1)* n + j))
        return ufs.count
```