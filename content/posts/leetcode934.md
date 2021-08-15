---
title: "934 最短的桥"
date: 2021-08-14T09:28:39+08:00
draft: false
tags: ["算法", "BFS", "DFS", "五星"]
categories: ["技术"]
---

**题目**

在给定的二维二进制数组 A 中，存在两座岛。（岛是由四面相连的 1 形成的一个最大组。）

现在，我们可以将 0 变为 1，以使两座岛连接起来，变成一座岛。

返回必须翻转的 0 的最小数目。（可以保证答案至少是 1 。）

示例 1：
```
输入：A = [[0,1],[1,0]]
输出：1
```

链接：https://leetcode-cn.com/problems/shortest-bridge

**解题思路**

首先找到第一座岛的边界，，将它不断向外延伸一圈，直到到达了另一座岛。

在寻找这座岛时，我们使用深度优先搜索。在向外延伸时，我们使用广度优先搜索。


**DFS + BFS**

```python
class Solution(object):

    def isBorder(self, grid, m, n, i, j):
        if i == 0 or i == m-1 or j == 0 or j == n-1:
            return True
        for di, dj in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
            newi = i + di
            newj = j + dj
            if grid[newi][newj] == 0:
                return True
        return False

    def shortestBridge(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        m = len(grid)
        n = len(grid[0])
        visited = [[0] * n for i in range(m)]
        stack = []
        q = deque([])
        step = 0
        
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1 and visited[i][j] == 0:
                    stack.append((i, j))
                    visited[i][j] = 1
                    while stack:
                        curi, curj = stack.pop()
                        if self.isBorder(grid, m, n, curi, curj):
                            q.append((curi, curj))
                        for di, dj in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                            newi = curi + di
                            newj = curj + dj
                            if 0 <= newi < m and 0 <= newj < n and grid[newi][newj] == 1 and visited[newi][newj] == 0:
                                stack.append((newi, newj))
                                visited[newi][newj] = 1
                    # bfs
                    while q:
                        for _ in range(len(q)):
                            curi, curj = q.popleft()
                            for di, dj in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                                newi = curi + di
                                newj = curj + dj
                                if 0 <= newi < m and 0 <= newj < n and visited[newi][newj] == 0:
                                    if grid[newi][newj] == 1:
                                        return step
                                    else:
                                        q.append((newi, newj))
                                        visited[newi][newj] = 1
                        step += 1
                    return step
```