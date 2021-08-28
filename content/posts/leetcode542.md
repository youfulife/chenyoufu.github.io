---
title: "542 01矩阵"
date: 2021-08-28T20:41:44+08:00
draft: false
tags: ["算法", "BFS", "五星"]
categories: ["技术"]
---

**题目**

给定一个由 0 和 1 组成的矩阵 mat ，请输出一个大小相同的矩阵，其中每一个格子是 mat 中对应位置元素到最近的 0 的距离。

两个相邻元素间的距离为 1 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/01-matrix
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**解题思路**

这个题目和橘子题一样，属于多源BFS。

```python
class Solution(object):

    def updateMatrix(self, mat):
        m = len(mat)
        n = len(mat[0])
        ans = [[0]* n for _ in range(m)]
        visited = [[0]* n for _ in range(m)]

        q = deque([])
        # 所有0 加入队列
        for i in range(m):
            for j in range(n):
                if mat[i][j] == 0:
                    q.append((i, j))
                    visited[i][j] = 1
        # 记录每一圈bfs的层数
        step = 0
        while q:
            qsize = len(q)
            for _ in range(qsize):
                i, j = q.popleft()
                # 更新结果
                ans[i][j] = step
                for di, dj in [[1, 0], [-1, 0], [0, -1], [0, 1]]:
                    newi = di + i
                    newj = dj + j
                    if 0 <= newi < m and 0 <= newj < n and visited[newi][newj] == 0 and mat[newi][newj] == 1:
                        q.append((newi, newj))
                        visited[newi][newj] = 1
            step += 1
        return ans
```