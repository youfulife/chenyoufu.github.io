---
title: "279 完全平方数"
date: 2021-08-20T07:20:31+08:00
draft: false
tags: ["算法", "图", "BFS", "五星", "动态规划"]
categories: ["技术"]
---

**题目**

给定正整数 n，找到若干个完全平方数（比如 1, 4, 9, 16, ...）使得它们的和等于 n。你需要让组成和的完全平方数的个数最少。

给你一个整数 n ，返回和为 n 的完全平方数的 最少数量 。

完全平方数 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，1、4、9 和 16 都是完全平方数，而 3 和 11 不是。


示例 1：
```
输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
```
示例 2：
```
输入：n = 13
输出：2
解释：13 = 4 + 9
```

链接：https://leetcode-cn.com/problems/perfect-squares

**解题思路**

**BFS**

```python
class Solution(object):
    def numSquares(self, n):
        q = deque([n])
        step = 1

        while q:
            qsize = len(q)
            for _ in range(qsize):
                i = q.popleft()
                j = 1
                while j * j < i:
                    q.append(i - j * j)
                    j += 1
                if j * j == i:
                    return step
            step += 1
        return -1
```

**动态规划**