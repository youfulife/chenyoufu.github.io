---
title: "130 被围绕的区域"
date: 2021-08-13T06:42:51+08:00
draft: false
tags: ["算法", "BFS", "并查集", "五星"]
categories: ["技术"]
---

**题目**

给你一个 m x n 的矩阵 board ，由若干字符 'X' 和 'O' ，找到所有被 'X' 围绕的区域，并将这些区域里所有的 'O' 用 'X' 填充。

示例 1：

```
输入：board = [["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]
输出：[["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]
解释：被围绕的区间不会存在于边界上，换句话说，任何边界上的 'O' 都不会被填充为 'X'。 任何不在边界上，或不与边界上的 'O' 相连的 'O' 最终都会被填充为 'X'。如果两个元素在水平或垂直方向相邻，则称它们是“相连”的。
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/surrounded-regions
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**解题思路**

对于每一个边界上的 O，我们以它为起点，标记所有与它直接或间接相连的字母 O；

最后我们遍历这个矩阵，对于每一个字母：
* 如果该字母被标记过，则该字母为没有被字母 X 包围的字母 O，我们将其还原为字母 O；
* 如果该字母没有被标记过，则该字母为被字母 X 包围的字母 O，我们将其修改为字母 X。


**BFS**

还可以在原数组中将跟边界相连的O修改成别的值，然后再遍历的时候修改过来，可以不需要visited数组，节省空间。

```python
class Solution(object):

    def solve(self, board):
        """
        :type board: List[List[str]]
        :rtype: None Do not return anything, modify board in-place instead.
        """
        m = len(board)
        n = len(board[0])
        visited = [[0]*n for i in range(m)]

        for i in range(m):
            for j in range(n):
                if board[i][j] == 'O' and (i == 0 or i == m-1 or j == 0 or j == n-1) and visited[i][j] == 0:
                    visited[i][j] = 1
                    q = deque([(i, j)])
                    while q:
                        
                        curi, curj = q.popleft()
                        for di, dj in [(1, 0), (0, 1), (-1, 0), (0, -1)]:
                            x = curi + di
                            y = curj + dj
                            if 0 <= x < m and 0 <= y < n and visited[x][y] == 0 and board[x][y] == 'O':
                                q.append((x, y))
                                visited[x][y] = 1
        for i in range(m):
            for j in range(n):
                if board[i][j] == 'O' and visited[i][j] == 0:
                    board[i][j] = 'X'
        return
```

**并查集**

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
            root_x = self.find(x)
            root_y = self.find(y)
            if root_x == root_y:
                return
            self.p[root_x] = root_y
            self.count -= 1
            return
        
        def isConnected(self, x, y):
            return self.find(x) == self.find(y)


    def solve(self, board):
        """
        :type board: List[List[str]]
        :rtype: None Do not return anything, modify board in-place instead.
        """
        m = len(board)
        n = len(board[0])
        count = m * n + 1
        ufs = Solution.UFS(count)

        for i in range(m):
            for j in range(n):
                if board[i][j] == "O":
                    for di, dj in [(1, 0), (0, 1)]:
                        curi = i + di
                        curj = j + dj
                        if 0<=curi < m and 0<=curj < n and board[curi][curj] == "O":
                            ufs.union(i * n + j, curi * n + curj)
        
        for i in range(m):
            for j in [0, n-1]:
                if board[i][j] == "O":
                    ufs.union(i * n + j, m * n)
        
        for i in [0, m-1]:
            for j in range(n):
                if board[i][j] == "O":
                    ufs.union(i * n + j, m * n)
        
        for i in range(m):
            for j in range(n):
                if board[i][j] == "O" and not ufs.isConnected(i * n + j, m * n):
                    board[i][j] = "X"
        return
```