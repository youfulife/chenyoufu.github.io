---
title: "岛屿问题"
date: 2022-02-02T12:24:29+08:00
draft: false
tags: ["算法", "BFS", "DFS", "并查集"]
categories: ["技术"]
---

**岛屿问题要点总结**

* 二维平面的方向坐标遍历以及边界条件
* visited数组，二维或者一维的初始化
* 可以优先考虑BFS，更容易理解
* DFS的代码更加简洁清晰
* 并查集对于连通问题逻辑更加清晰


**题目**

695. 岛屿的最大面积

给你一个大小为 m x n 的二进制矩阵 grid 。

岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在 水平或者竖直的四个方向上 相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。

岛屿的面积是岛上值为 1 的单元格的数目。

计算并返回 grid 中最大的岛屿面积。如果没有岛屿，则返回面积为 0 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/max-area-of-island
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



```python
class Solution(object):
    def maxAreaOfIsland(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        m = len(grid)
        n = len(grid[0])
        visited = [[0] * n for _ in range(m)]

        def bfs(x, y):
            area = 0
            q = [[x, y]]
            for i, j in q:
                area += 1
                for di, dj in [[-1, 0],[1, 0],[0,-1], [0, 1]]:
                    newi, newj = i + di, j + dj
                    if 0<=newi<m and 0<=newj<n and visited[newi][newj] == 0 and grid[newi][newj] == 1:
                        visited[newi][newj] = 1
                        q.append([newi, newj])
            return area
                    
        ans = 0
        for i in range(m):
            for j in range(n):
                if visited[i][j] == 0 and grid[i][j] == 1:
                    visited[i][j] = 1
                    ans = max(ans, bfs(i, j))
        return ans
```

417. 太平洋大西洋水流问题

给定一个 m x n 的非负整数矩阵来表示一片大陆上各个单元格的高度。“太平洋”处于大陆的左边界和上边界，而“大西洋”处于大陆的右边界和下边界。

规定水流只能按照上、下、左、右四个方向流动，且只能从高到低或者在同等高度上流动。

请找出那些水流既可以流动到“太平洋”，又能流动到“大西洋”的陆地单元的坐标。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/pacific-atlantic-water-flow
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def pacificAtlantic(self, heights):
        """
        :type heights: List[List[int]]
        :rtype: List[List[int]]
        """
        m = len(heights)
        n = len(heights[0])
        
        visited = [[0] * n for _ in range(m)]
        p = [[0] * n for _ in range(m)]
        a = [[0] * n for _ in range(m)]
        ans = []

        def dfs(i, j, grid): 
            grid[i][j] = 1

            for dx, dy in [(0, -1), (0, 1), (1, 0), (-1, 0)]:
                newi, newj = i + dx, j + dy
                
                if 0<=newi<m and 0<=newj<n and grid[newi][newj] == 0 and heights[i][j] <= heights[newi][newj]:
                    dfs(newi, newj, grid)
        
        for i in range(m):
            dfs(i, 0, p)
            dfs(i, n-1, a)
        for j in range(n):
            dfs(0, j, p)
            dfs(m-1, j, a)
        
        for i in range(m):
            for j in range(n):
                if p[i][j] == 1 and a[i][j] == 1:
                    ans.append([i, j])
        return ans
```

```python
class Solution(object):
    def pacificAtlantic(self, heights):
        """
        :type heights: List[List[int]]
        :rtype: List[List[int]]
        """
        m = len(heights)
        n = len(heights[0])

        p = [[0] * n for _ in range(m)]
        a = [[0] * n for _ in range(m)]

        def bfs(i, j, grid):
            grid[i][j] = 1
            q = [(i, j)]
            for x, y in q:
                for dx, dy in [[-1, 0], [1, 0], [0, -1], [0, 1]]:
                    newx, newy = x + dx, y+dy
                    if 0<=newx<m and 0<=newy<n and grid[newx][newy] == 0 and heights[newx][newy] >= heights[x][y]:
                        grid[newx][newy] = 1
                        q.append((newx, newy))
            return
        
        ans = []
        for i in range(m):
            bfs(i, 0, p)
            bfs(i, n-1, a)
        
        for j in range(n):
            bfs(0, j, p)
            bfs(m-1, j, a)
        
        for i in range(m):
            for j in range(n):
                if p[i][j] == 1 and a[i][j] == 1:
                    ans.append([i, j])
        return ans
```

130. 被围绕的区域

给你一个 m x n 的矩阵 board ，由若干字符 'X' 和 'O' ，找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。

被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/surrounded-regions
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def solve(self, board):
        """
        :type board: List[List[str]]
        :rtype: None Do not return anything, modify board in-place instead.
        """
        m = len(board)
        n = len(board[0])
        visited = [[0] * n for _ in range(m)]

        def dfs(i, j):
            visited[i][j] = 1

            for x, y in [(-1, 0), (0, -1), (0, 1), (1, 0)]:
                newi = i +x
                newj = j +y
                if 0<=newi<m and 0<=newj<n and visited[newi][newj] == 0 and board[newi][newj] == 'O':
                    dfs(newi, newj)

            return
        
        for i in range(m):
            for j in range(n):
                if (i==0 or i==m-1 or j == 0 or j == n-1) and board[i][j] == 'O' and visited[i][j] == 0:
                    dfs(i, j)
        
        for i in range(m):
            for j in range(n):
                if visited[i][j] == 0 and board[i][j] == 'O':
                    board[i][j] = 'X'
```

```python
class Solution(object):
    def solve(self, board):
        """
        :type board: List[List[str]]
        :rtype: None Do not return anything, modify board in-place instead.
        """
        m = len(board)
        n = len(board[0])
        q = []
        for i in range(m):
            for j in [0, n-1]:
                if board[i][j] == 'O':
                    q.append((i, j))
        
        for i in [0, m-1]:
            for j in range(n):
                if board[i][j] == 'O':
                    q.append((i, j))
        
        visited = [[0] * n for _ in range(m)]
        for i, j in q:
            visited[i][j] = 1
            for di, dj in [(0, -1), (0, 1), (-1, 0), (1, 0)]:
                newi, newj = i + di, j + dj
                if 0<=newi<m and 0<=newj<n and visited[newi][newj] == 0 and board[newi][newj] == 'O':
                    visited[newi][newj] = 1
                    q.append((newi, newj))
        
        for i in range(m):
            for j in range(n):
                if visited[i][j] == 0 and board[i][j] == 'O':
                    board[i][j] = 'X'
        return
```

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
        
        def isConnected(self, x, y):
            return self.find(x) == self.find(y)

    def solve(self, board):
        """
        :type board: List[List[str]]
        :rtype: None Do not return anything, modify board in-place instead.
        """
        m = len(board)
        n = len(board[0])
        ufs = Solution.UFS(m * n+1)

        for i in range(m):
            for j in range(n):
                if board[i][j] == 'O':
                    if i == 0 or i == m-1 or j == 0 or j == n-1:
                        ufs.union(i *n + j, m * n)
                    for x, y in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                        newi = i + x
                        newj = j + y
                        if 0<=newi<m and 0<=newj<n and board[newi][newj] == 'O':
                            ufs.union(i *n + j, newi *n +newj)
        
        for i in range(m):
            for j in range(n):
                if board[i][j] == 'O' and not ufs.isConnected(i * n + j, m *n):
                    board[i][j] = 'X'
        return
```

934. 最短的桥

在给定的二维二进制数组 A 中，存在两座岛。（岛是由四面相连的 1 形成的一个最大组。）

现在，我们可以将 0 变为 1，以使两座岛连接起来，变成一座岛。

返回必须翻转的 0 的最小数目。（可以保证答案至少是 1 。）

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shortest-bridge
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**思路**

1. 找到其中一个桥，做好标记
2. 将另一个桥的所有点加入到队列中，BFS开始扩散，每次+1

```python
class Solution(object):
    def shortestBridge(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        m = len(grid)
        n = len(grid[0])
        visited = [[0] * n for _ in range(m)]

        q = []
        found = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    q.append((i, j))
                    found = 1
                    break
            if found==1:
                break

        # 第一个岛的所有点标记为1
        for i, j in q:
            visited[i][j] = 1
            for di, dj in [(0, 1), (0, -1), (1, 0), (-1, 0)]:
                newi, newj = i+di, j+dj
                if 0<=newi<m and 0<=newj<n and visited[newi][newj] == 0 and grid[newi][newj] == 1:
                    visited[newi][newj] = 1
                    q.append((newi, newj))
        
        # 另一个桥的所有点加入到队列中
        q = collections.deque([])
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1 and visited[i][j] == 0:
                    q.append((i, j))
                    visited[i][j] = 2
        # BFS 扩散
        step = 0
        while q:
            for _ in range(len(q)):
                i, j = q.popleft()
                for di, dj in [(0, 1), (0, -1), (1, 0), (-1, 0)]:
                    newi, newj = i+di, j+dj
                    if 0<=newi<m and 0<=newj<n:
                        if visited[newi][newj] == 1:
                            return step
                        if visited[newi][newj] == 0:
                            visited[newi][newj] = 2
                            q.append((newi, newj))
            step += 1
        return
```

1034. 边界着色

给你一个大小为 m x n 的整数矩阵 grid ，表示一个网格。另给你三个整数 row、col 和 color 。网格中的每个值表示该位置处的网格块的颜色。

两个网格块属于同一 连通分量 需满足下述全部条件：

两个网格块颜色相同
在上、下、左、右任意一个方向上相邻
连通分量的边界 是指连通分量中满足下述条件之一的所有网格块：

在上、下、左、右任意一个方向上与不属于同一连通分量的网格块相邻
在网格的边界上（第一行/列或最后一行/列）
请你使用指定颜色 color 为所有包含网格块 grid[row][col] 的 连通分量的边界 进行着色，并返回最终的网格 grid 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/coloring-a-border
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def colorBorder(self, grid, row, col, color):
        """
        :type grid: List[List[int]]
        :type row: int
        :type col: int
        :type color: int
        :rtype: List[List[int]]
        """
        m = len(grid)
        n = len(grid[0])
        visited = [[0]*n for _ in range(m)]

        def isboard(i, j):
            if i == 0 or i == m-1 or j == 0 or j == n-1:
                return True
            for dx,dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                newi, newj = i+dx, j+dy
                if grid[newi][newj] != grid[i][j]:
                    return True
            return False
            

        def dfs(i, j, path):
            if isboard(i, j):
                path.append((i, j))
            for dx,dy in [(-1, 0), (1, 0), (0, -1), (0, 1)]:
                newi, newj = i+dx, j+dy
                if 0<=newi<m and 0<=newj<n and visited[newi][newj]==0 and grid[newi][newj] == grid[i][j]:
                    visited[newi][newj]=1
                    dfs(newi, newj, path)
            return
        path = []
        visited[row][col] = 1
        dfs(row, col, path)
        for i, j in path:
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