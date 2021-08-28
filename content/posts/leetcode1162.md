---
title: "1162 地图分析"
date: 2021-08-28T21:05:26+08:00
draft: false
tags: ["算法", "BFS"]
categories: ["技术"]
---

**题目**

你现在手里有一份大小为 N x N 的 网格 grid，上面的每个 单元格 都用 0 和 1 标记好了。其中 0 代表海洋，1 代表陆地，请你找出一个海洋单元格，这个海洋单元格到离它最近的陆地单元格的距离是最大的。

我们这里说的距离是「曼哈顿距离」（ Manhattan Distance）：(x0, y0) 和 (x1, y1) 这两个单元格之间的距离是 |x0 - x1| + |y0 - y1| 。

如果网格上只有陆地或者海洋，请返回 -1。
 
```
输入：[[1,0,1],[0,0,0],[1,0,1]]
输出：2
解释： 
海洋单元格 (1, 1) 和所有陆地单元格之间的距离都达到最大，最大距离为 2。
```

链接：https://leetcode-cn.com/problems/as-far-from-land-as-possible

**解题思路**

```python
class Solution(object):
    def maxDistance(self, grid):
        n = len(grid)
        visited = [[0] * n for _ in range(n)]
        nums0 = 0
        nums1 = 0
        q = deque([])
        for i in range(n):
            for j in range(n):
                if grid[i][j] == 1:
                    nums1 += 1
                    q.append((i, j))
                else:
                    nums0 += 1
        if nums0 == 0 or nums1 == 0:
            return -1

        step = 0
        while q:
            for _ in range(len(q)):
                i, j = q.popleft()
                for di, dj in [[0, 1], [0, -1], [1, 0], [-1, 0]]:
                    ii = i + di
                    jj = j + dj
                    if 0 <= ii < n and 0<=jj<n and visited[ii][jj] == 0 and grid[ii][jj] == 0:
                        q.append((ii, jj))
                        visited[ii][jj] = 1
            step += 1
        # 注意边界
        return step - 1
```
