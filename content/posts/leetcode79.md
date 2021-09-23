---
title: "79 单词搜索"
date: 2021-09-23T22:52:38+08:00
draft: false
tags: ["算法", "回溯", "DFS"]
categories: ["技术"]
---

**题目**

给定一个 m x n 二维字符网格 board 和一个字符串单词 word 。如果 word 存在于网格中，返回 true ；否则，返回 false 。

单词必须按照字母顺序，通过相邻的单元格内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母不允许被重复使用。

```
输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/word-search
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**解题思路**

DFS

```python
class Solution(object):
    def exist(self, board, word):
        m = len(board)
        n = len(board[0])
        visited = [[0]* n for _ in range(m)]
        l = [(i, j) for i in range(m) for j in range(n) if board[i][j] == word[0]]

        def dfs(i, j, path):
            if path == word:
                return True
            
            for dx, dy in [[-1, 0], [1, 0], [0, -1], [0, 1]]:
                newi = i + dx
                newj = j + dy
                if 0<=newi<m and 0<=newj<n and visited[newi][newj] == 0: 
                    x = board[newi][newj]
                    if x == word[len(path)]:
                        visited[newi][newj] = 1
                        if dfs(newi, newj, path + board[newi][newj]):
                            return True
                        visited[newi][newj] = 0
            return False
        
        for i, j in l:
            visited[i][j] = 1
            if dfs(i, j, word[0]):
                return True
            visited[i][j] = 0
        return False
```