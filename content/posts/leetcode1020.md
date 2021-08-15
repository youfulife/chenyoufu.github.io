---
title: "1020 飞地的数量"
date: 2021-08-14T23:08:06+08:00
draft: false
tags: ["算法", "BFS", "并查集", "DFS"]
categories: ["技术"]
---

**题目**

给出一个二维数组 A，每个单元格为 0（代表海）或 1（代表陆地）。

移动是指在陆地上从一个地方走到另一个地方（朝四个方向之一）或离开网格的边界。

返回网格中无法在任意次数的移动中离开网格边界的陆地单元格的数量。


示例 1：
```
输入：[[0,0,0,0],[1,0,1,0],[0,1,1,0],[0,0,0,0]]
输出：3
解释： 
有三个 1 被 0 包围。一个 1 没有被包围，因为它在边界上。
```

链接：https://leetcode-cn.com/problems/number-of-enclaves

**解题思路**

不与四条边相连的1的个数。

**并查集**


**BFS**

```python
class Solution(object):
    def numEnclaves(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        m = len(grid)
        n = len(grid[0])
        visited = [[0]*n for i in range(m)]
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1 and visited[i][j] == 0 and (i == 0 or i == m-1 or j==0 or j==n-1):
                    q = deque([(i, j)])
                    visited[i][j] = 1
                    while q:
                        curi, curj = q.popleft()
                        for di, dj in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                            newi = curi + di
                            newj = curj + dj
                            if 0<=newi<m and 0<=newj<n and visited[newi][newj] == 0 and grid[newi][newj] == 1:
                                q.append((newi, newj))
                                visited[newi][newj] = 1
        ans = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1 and visited[i][j] == 0:
                    ans += 1
        return ans
```

**DFS**