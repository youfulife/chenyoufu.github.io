---
title: "剑指 Offer 13 机器人的运动范围"
date: 2021-08-16T22:20:52+08:00
draft: false
tags: ["算法", "BFS", "DFS"]
categories: ["技术"]
---

**题目**

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

示例 1：
```
输入：m = 2, n = 3, k = 1
输出：3
```
示例 2：
```
输入：m = 3, n = 1, k = 0
输出：1
```

链接：https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof

**解题思路**

```python
class Solution(object):
    def movingCount(self, m, n, k):
        def bitSum(i, j):
            s = 0
            while i > 0:
                s += i % 10
                i = i / 10
            while j > 0:
                s += j % 10
                j = j / 10
            return s 


        q = [[0, 0]]
        visited = set([(0, 0)])

        ans = 0
        for r0, c0 in q:
            ans += 1
            for i, j in [[-1, 0], [1, 0], [0, -1], [0, 1]]:
                r = r0 + i
                c = c0 + j
                if 0 <= r < m and 0 <= c < n and (r, c) not in visited and bitSum(r, c) <= k:
                    q.append([r, c])
                    visited.add((r, c))
        return ans
```