---
title: "1034 边框着色"
date: 2021-08-15T19:57:40+08:00
draft: false
tags: ["算法", "BFS", "DFS"]
categories: ["技术"]
---

**题目**

给出一个二维整数网格 grid，网格中的每个值表示该位置处的网格块的颜色。

只有当两个网格块的颜色相同，而且在四个方向中任意一个方向上相邻时，它们属于同一连通分量。

连通分量的边界是指连通分量中的所有与不在分量中的正方形相邻（四个方向上）的所有正方形，或者在网格的边界上（第一行/列或最后一行/列）的所有正方形。

给出位于 (r0, c0) 的网格块和颜色 color，使用指定颜色 color 为所给网格块的连通分量的边界进行着色，并返回最终的网格 grid 。

示例 1：
```
输入：grid = [[1,1],[1,2]], r0 = 0, c0 = 0, color = 3
输出：[[3, 3], [3, 2]]
```

链接：https://leetcode-cn.com/problems/coloring-a-border

**解题思路**

**BFS**

bfs 找到目标连通分量的边界，保存起来，遍历完成后修改边界元素。

```python
class Solution(object):
    
    def colorBorder(self, grid, row, col, color):

        def isBorder(grid, m, n, i, j):
            if i == 0 or i == m-1 or j == 0 or j == n-1:
                return True
            target = grid[i][j]
            if grid[i-1][j] != target or grid[i+1][j] != target or grid[i][j-1] != target or grid[i][j+1] != target:
                return True
            return False
            

        m = len(grid)
        n = len(grid[0])

        visited = [[0]* n for i in range(m)]
        target = grid[row][col]
        border = []

        q = deque([(row, col)])
        visited[row][col] = 1
        while q:
            curi, curj = q.popleft()
            if isBorder(grid, m, n, curi, curj):
                border.append((curi, curj))
            for di, dj in [(-1, 0), (1, 0), (0, 1), (0, -1)]:
                newi = di + curi
                newj = dj + curj
                if 0 <= newi < m and 0 <= newj < n and grid[newi][newj] == target and visited[newi][newj] == 0:
                    q.append((newi, newj))
                    visited[newi][newj] = 1

        for i, j in border:
            grid[i][j] = color
        return grid
```

上述代码还是看着非常繁琐。

https://leetcode.com/problems/coloring-a-border/discuss/282839/Python-BFS-and-DFS

```python
class Solution(object):
    def colorBorder(self, grid, r0, c0, color):
        m, n = len(grid), len(grid[0])
        # 基于set来，比visited数组可以更加节省空间
        bfs, component, border = [[r0, c0]], set([(r0, c0)]), set()
        for r0, c0 in bfs:
            for i, j in [[0, 1], [1, 0], [-1, 0], [0, -1]]:
                r, c = r0 + i, c0 + j
                if 0 <= r < m and 0 <= c < n and grid[r][c] == grid[r0][c0]:
                    if (r, c) not in component:
                        bfs.append([r, c])
                        component.add((r, c))
                else: # 边界
                    border.add((r0, c0))
        for x, y in border: grid[x][y] = color
        return grid
```

**DFS**

```python
    def colorBorder(self, grid, r0, c0, color):
        seen, m, n = set(), len(grid), len(grid[0])

        def dfs(x, y):
            if (x, y) in seen: return True
            if not (0 <= x < m and 0 <= y < n and grid[x][y] == grid[r0][c0]):
                return False
            seen.add((x, y))
            if dfs(x + 1, y) + dfs(x - 1, y) + dfs(x, y + 1) + dfs(x, y - 1) < 4:
                grid[x][y] = color
            return True
        dfs(r0, c0)
        return grid
```