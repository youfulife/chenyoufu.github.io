---
title: "463 岛屿的周长"
date: 2021-08-15T22:11:08+08:00
draft: flase
tags: ["算法", "BFS", "DFS"]
categories: ["技术"]
---

**题目**

给定一个 row x col 的二维网格地图 grid ，其中：grid[i][j] = 1 表示陆地， grid[i][j] = 0 表示水域。

网格中的格子 水平和垂直 方向相连（对角线方向不相连）。整个网格被水完全包围，但其中恰好有一个岛屿（或者说，一个或多个表示陆地的格子相连组成的岛屿）。

岛屿中没有“湖”（“湖” 指水域在岛屿内部且不和岛屿周围的水相连）。格子是边长为 1 的正方形。网格为长方形，且宽度和高度均不超过 100 。计算这个岛屿的周长。

```
输入：grid = [[0,1,0,0],[1,1,1,0],[0,1,0,0],[1,1,0,0]]
输出：16
解释：它的周长是上面图片中的 16 个黄色的边
```
示例 2：
```
输入：grid = [[1]]
输出：4
```
示例 3：
```
输入：grid = [[1,0]]
输出：4
```

链接：https://leetcode-cn.com/problems/island-perimeter

**解题思路**

直接遍历所有元素，看1的格子是不是边界，将与水或者岸边相连的边 +1就可以

```python
class Solution(object):
    def islandPerimeter(self, grid):
        rows = len(grid)
        cols = len(grid[0])
        ans = 0
        for r0 in range(rows):
            for c0 in range(cols):
                if grid[r0][c0] == 1:
                    for i, j in [[-1, 0], [1, 0], [0, -1], [0, 1]]:
                        r = r0 + i
                        c = c0 + j
                        if 0 <= r < rows and 0 <= c < cols and grid[r][c] == 1:
                            pass
                        else: # 边界
                            ans += 1
        return ans
```


英文站讨论区思路

https://leetcode.com/problems/island-perimeter/discuss/95001/clear-and-easy-java-solution

1. loop over the matrix and count the number of islands;
2. if the current dot is an island, count if it has any right neighbour or down neighbour;
3. the result is islands * 4 - neighbours * 2

```python
class Solution(object):
    def islandPerimeter(self, grid):
        rows = len(grid)
        cols = len(grid[0])
        island = 0
        neighbour = 0
        for r0 in range(rows):
            for c0 in range(cols):
                if grid[r0][c0] == 1:
                    island += 1
                    for i, j in [[1, 0], [0, 1]]:
                        r = r0 + i
                        c = c0 + j
                        if 0 <= r < rows and 0 <= c < cols and grid[r][c] == 1:
                            neighbour += 1
        return island * 4 - neighbour * 2
```