---
title: "200 岛屿数量"
date: 2021-07-28T23:02:47+08:00
draft: false
tags: ["算法", "并查集", "五星", "DFS", "BFS"]
categories: ["技术"]
---
**题目**

给你一个由 '1'（陆地）和 '0'（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

示例 1：
```
输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
```
示例 2：
```
输入：grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
输出：3
```
链接：https://leetcode-cn.com/problems/number-of-islands

**解题思路**

**DFS**

```python
class Solution(object):
    def numIslands(self, grid):
        m = len(grid)
        n = len(grid[0])
        visited = [[0] * n for _ in range(m)]

        def dfs(i, j):
            for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                newi = dx + i
                newj = dy + j
                if 0<=newi<m and 0<=newj<n and visited[newi][newj] == 0 and grid[newi][newj] == '1':
                    visited[newi][newj] = 1
                    dfs(newi, newj)
        
        ans = 0
        for i in range(m):
            for j in range(n):
                if visited[i][j] == 0 and grid[i][j] == '1':
                    visited[i][j] = 1
                    dfs(i, j)
                    ans += 1
        return ans
```

**BFS**

```python
class Solution(object):
    def bfs(self, grid, i, j, m, n, visited):
        # 入队列的是坐标
        q = collections.deque([(i, j)])
        # 这里不要忘记
        visited[i][j] = 1

        while q:
            curi, curj = q.popleft()
            for di, dj in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                x = curi + di
                y = curj + dj
                # 不要忘记判断边界
                if 0 <= x < m and 0 <= y < n and grid[x][y] == '1' and visited[x][y] == 0:
                    q.append((x, y))
                    # 这里不要忘记标记为已经访问过
                    visited[x][y] = 1
        return

    def numIslands(self, grid):
        m = len(grid)
        n = len(grid[0])
        # 二维数组初始化方法
        visited = [[0]* n for i in range(m)]

        ans = 0
        for i in range(m):
            for j in range(n):
                # 这里 '1' 是字符串, 不是整数 1
                if grid[i][j] == '1' and visited[i][j] == 0:
                    self.bfs(grid, i, j, m, n, visited)
                    ans += 1
        return ans
```

2021.9.25 更新复习

```python
class Solution(object):
    def numIslands(self, grid):
        m = len(grid)
        n = len(grid[0])
        visited = [[0] * n for _ in range(m)]

        def bfs(i, j):
            q = [(i, j)]
            for i, j in q:
                for dx, dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                    newi = dx + i
                    newj = dy + j
                    if 0<=newi<m and 0<=newj<n and visited[newi][newj] == 0 and grid[newi][newj] == '1':
                        visited[newi][newj] = 1
                        q.append((newi, newj))
        
        ans = 0
        for i in range(m):
            for j in range(n):
                if visited[i][j] == 0 and grid[i][j] == '1':
                    visited[i][j] = 1
                    bfs(i, j)
                    ans += 1
        return ans
```

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
        
        def size(self):
            cnt = 0
            for i in range(self.n):
                if self.parent[i] == i:
                    cnt += 1
            return cnt


    def numIslands(self, grid):
        m = len(grid)
        n = len(grid[0])
        ufs = Solution.UFS(m * n)
        for i in range(m):
            for j in range(n):
                # 题目给出的是"1" 是字符串，不是整数，第一次写成了整数，所有流程都走了else
                # grid[i][j] 对应的并查集parent的下标为 i * n + j， 一开始写成了i * m + j 花了好长时间调试
                if grid[i][j] == "1":
                    # 上
                    if i > 0 and grid[i-1][j] == "1":
                        ufs.union(i * n + j, (i-1) * n + j)
                    # 左
                    if j > 0 and grid[i][j-1] == "1":
                        ufs.union(i * n + j, i * n + j - 1)
                    # 下
                    if i < m - 1 and grid[i + 1][j] == "1":
                        ufs.union(i * n + j, (i + 1) * n + j)
                    #右
                    if j < n - 1 and grid[i][j+1] == "1":
                        ufs.union(i * n + j, i * n + j + 1)
                    
                    grid[i][j] = 0
                else:
                    # 设置为-1，用来区分原来就是0的
                    ufs.parent[i * n + j] = -1
        return ufs.size()
```

**代码优化，过程中不需要修改grid的值和grid=0的parent的值**

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
        
        def size(self):
            cnt = 0
            for i in range(self.n):
                if self.parent[i] == i:
                    cnt += 1
            return cnt


    def numIslands(self, grid):
        m = len(grid)
        n = len(grid[0])
        ufs = Solution.UFS(m * n)
        zeros = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == "1":
                    # 只需要向下和向右看
                    # 下
                    if i < m - 1 and grid[i + 1][j] == "1":
                        ufs.union(i * n + j, (i + 1) * n + j)
                    #右
                    if j < n - 1 and grid[i][j+1] == "1":
                        ufs.union(i * n + j, i * n + j + 1)
                else:
                    # 记住 水 的个数
                    zeros += 1
        return ufs.size() - zeros
```

2021.9.25 更新复习

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

    def numIslands(self, grid):
        m = len(grid)
        n = len(grid[0])        
        zeros = 0
        ans = 0
        ufs = Solution.UFS(m * n)
        for i in range(m):
            for j in range(n):
                if grid[i][j] == '0':
                    zeros += 1
                else:
                    for dx, dy in [(1, 0), (0, 1)]:
                        newi, newj = i + dx, j + dy
                        if 0 <= newi < m and 0<=newj<n and grid[newi][newj] == '1':
                            ufs.union(i * n + j, newi * n + newj)
        return ufs.count - zeros  
```