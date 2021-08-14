---
title: "417 太平洋大西洋水流问题"
date: 2021-08-13T06:38:44+08:00
draft: false
tags: ["算法", "BFS", "五星"]
categories: ["技术"]
---

**题目**

给定一个 m x n 的非负整数矩阵来表示一片大陆上各个单元格的高度。“太平洋”处于大陆的左边界和上边界，而“大西洋”处于大陆的右边界和下边界。

规定水流只能按照上、下、左、右四个方向流动，且只能从高到低或者在同等高度上流动。

请找出那些水流既可以流动到“太平洋”，又能流动到“大西洋”的陆地单元的坐标。

示例：
```
给定下面的 5x5 矩阵:

  太平洋 ~   ~   ~   ~   ~ 
       ~  1   2   2   3  (5) *
       ~  3   2   3  (4) (4) *
       ~  2   4  (5)  3   1  *
       ~ (6) (7)  1   4   5  *
       ~ (5)  1   1   2   4  *
          *   *   *   *   * 大西洋

返回:

[[0, 4], [1, 3], [1, 4], [2, 2], [3, 0], [3, 1], [4, 0]] (上图中带括号的单元).
```

提示：

* 输出坐标的顺序不重要
* m 和 n 都小于150

链接：https://leetcode-cn.com/problems/pacific-atlantic-water-flow

**解题思路**

本题的关键在于起点的设置，如果把中间节点设置为起点，那么题目就会变得非常复杂。

我们可以将边界设置为BFS的起点。

我们可以先从太平洋出发，对每个没处理过的太平洋相邻的节点进行BFS遍历，满足梯度的要求，

通过一个辅助的visited数组进行记录，访问到每个节点，对每个节点进行 + inc

之后，我们从大西洋出发，对每个没处理过的大西洋相邻的节点进行BFS遍历，也要满足梯度的要求，

通过辅助的visited数组进行记录，对于访问到的每个几点，对每个节点进行 + inc。

上面两个过程的inc 值是不同的，我约定是一个+1 一个 +2。

当每个边界节点都作为起点进行过BFS，或者每个边界点都访问过后，

我们遍历visited， 找出其中值为3的节点，记录坐标，此坐标即为所求坐标。

参考：https://leetcode-cn.com/problems/pacific-atlantic-water-flow/solution/bfsbian-li-by-wkltljjs-c823/

```python
class Solution(object):

    def bfs(self, grid, i, j, m, n, visited, flag):
        q = deque([(i, j)])
        visited[i][j] += flag
        while q:
            curi, curj = q.popleft()

            for di, dj in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                newi, newj = curi + di, curj + dj
                if 0<=newi<m and 0<=newj<n and grid[curi][curj]<= grid[newi][newj] and visited[newi][newj] < flag:
                    q.append((newi, newj))
                    visited[newi][newj] += flag
        return


    def pacificAtlantic(self, grid):
        m = len(grid)
        n = len(grid[0])
        visited = [[0] * n for i in range(m)]
        ans = []

        flag = 1
        for i in range(m):
            if visited[i][0] < flag:
                self.bfs(grid, i, 0, m, n, visited, flag)
        for j in range(n):
            if visited[0][j] < flag:
                self.bfs(grid, 0, j, m, n, visited, flag)
        
        flag = 2
        for i in range(m):
            if visited[i][n-1] < flag:
                self.bfs(grid, i, n-1, m, n, visited, flag)
        for j in range(n):
            if visited[m-1][j] < flag:
                self.bfs(grid, m-1, j, m, n, visited, flag)

        for i in range(m):
            for j in range(n):
                if visited[i][j] == 3:
                    ans.append([i, j])
        return ans
```