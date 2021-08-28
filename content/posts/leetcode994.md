---
title: "994 腐烂的橘子"
date: 2021-08-28T19:20:01+08:00
draft: false
tags: ["算法", "BFS", "五星"]
categories: ["技术"]
---

**题目**

在给定的网格中，每个单元格可以有以下三个值之一：

值 0 代表空单元格；
值 1 代表新鲜橘子；
值 2 代表腐烂的橘子。
每分钟，任何与腐烂的橘子（在 4 个正方向上）相邻的新鲜橘子都会腐烂。

返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 -1。

链接：https://leetcode-cn.com/problems/rotting-oranges

**解题思路**

* 一开始，我们找出所有腐烂的橘子，将它们放入队列，作为第 0 层的结点。
* 然后进行 BFS 遍历，每个结点的相邻结点可能是上、下、左、右四个方向的结点，注意判断结点位于网格边界的特殊情况。
* 由于可能存在无法被污染的橘子，我们需要记录新鲜橘子的数量。在 BFS 中，每遍历到一个橘子（污染了一个橘子），就将新鲜橘子的数量减一。如果 BFS 结束后这个数量仍未减为零，说明存在无法被污染的橘子。


```python
class Solution(object):
    def orangesRotting(self, grid):
        q = deque([])
        # 记录新鲜橘子数量
        nums1 = 0

        m = len(grid)
        n = len(grid[0])
        # 找出所有腐烂橘子加入队列
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 2:
                    q.append((i, j))
                if grid[i][j] == 1:
                    nums1 += 1
        # 一开始就没有新鲜橘子，直接返回0
        if nums1 == 0:
            return 0
        # bfs
        t = -1
        while q:
            qsize = len(q)
            for _ in range(qsize):
                i, j = q.popleft()
                for di, dj in [[-1, 0], [1, 0], [0, 1], [0, -1]]:
                    newi = di + i
                    newj = dj + j
                    if 0 <= newi < m and 0<= newj <n and grid[newi][newj] == 1:
                        q.append((newi, newj))
                        grid[newi][newj] = 2
                        nums1 -= 1
            t += 1
        # bfs之后还有新鲜橘子返回-1                    
        return -1 if nums1 > 0 else t
```