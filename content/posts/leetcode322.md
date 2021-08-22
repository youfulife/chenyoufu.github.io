---
title: "322 零钱兑换"
date: 2021-06-08T21:31:56+08:00
draft: false
tags: ["算法", "动态规划", "BFS", "五星"]
categories: ["技术"]
---

**题目**

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

你可以认为每种硬币的数量是无限的。

示例 1：
```
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```
示例 2：
```
输入：coins = [2], amount = 3
输出：-1
```
示例 3：
```
输入：coins = [1], amount = 0
输出：0
```
链接：https://leetcode-cn.com/problems/coin-change

**解题思路**

* F(S)=F(S−C)+1

```python
class Solution(object):
    def coinChange(self, coins, amount):
        dp = [amount+1 for i in range(amount+1)]
        dp[0] = 0
        for i in range(0, amount+1):
            for coin in coins:
                if i - coin < 0:
                    continue
                dp[i] = min(dp[i], 1 + dp[i-coin])
        return dp[amount] if dp[amount] < amount+1 else -1
```

**BFS**


```python
class Solution(object):
    def coinChange(self, coins, amount):
        if amount == 0:
            return 0
        step = 0
        visited = {}
        q = deque([amount])
        visited[amount] = True

        while q:
            qsize = len(q)
            for _ in range(qsize):
                remain = q.popleft()
                if remain == 0:
                    return step 
                if remain > 0:
                    for c in coins:
                        x = remain - c
                        if x not in visited:
                            q.append(x)
                            visited[x] = True
            step += 1
        return -1
```