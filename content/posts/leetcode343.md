---
title: "Leetcode343"
date: 2021-09-27T22:19:31+08:00
draft: false
tags: ["算法", "数学", "动态规划"]
categories: ["技术"]
---

**题目**

给定一个正整数 n，将其拆分为至少两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。

示例 1:
```
输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
```
示例 2:
```
输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
```

说明: 你可以假设 n 不小于 2 且不大于 58。

链接：https://leetcode-cn.com/problems/integer-break

**解题思路**

**动态规划**

* dp[i]：分拆数字i，可以得到的最大乘积为dp[i]。
* 可以想 dp[i]最大乘积是怎么得到的呢？其实可以从1遍历j，然后有两种渠道得到dp[i].
* 一个是j * (i - j) 直接相乘。
* 一个是j * dp[i - j]，相当于是拆分(i - j)，对这个拆分不理解的话，可以回想dp数组的定义。
* 也可以这么理解，j * (i - j) 是单纯的把整数拆分为两个数相乘，而j * dp[i - j]是拆分成两个以及两个以上的个数相乘。
* dp[i] = max({dp[i], (i - j) * j, dp[i - j] * j});

```python
class Solution(object):
    def integerBreak(self, n):
        # dp[i] = max({dp[i], (i - j) * j, dp[i - j] * j})
        dp = [0] * (n+1)

        for i in range(2, n+1):
            for j in range(1, i):
                dp[i] = max(dp[i], dp[i-j] * j, (i-j) * j)
        return dp[n]
```

优化

```python
class Solution(object):
    def integerBreak(self, n):
        # dp[i] = max(dp[i-j] * j)
        # 将dp初始化为本身的值，相当于遍历的时候包含了 (i - j) * j
        dp = [i for i in range(n+1)]
        dp[n] = 0

        for i in range(2, n+1):
            for j in range(1, i):
                dp[i] = max(dp[i], dp[i-j] * j)
        return dp[n]
```

**数学**

来自 https://75.team/post/integer-break.html

定理： 任意两个不小于 2 的自然数 a、b，有 a + b ≤ a * b。

上面这个定理不难证明：假设 2 ≤ a ≤ b，那么有 a + b ≤ b + b ≤ 2 * b ≤ a * b

因此，从上面的定理得到——

推论：对于任意自然数 n， n ≥ 4，必然存在一个自然数拆分，将 n 拆分成两个自然数 a 和 b，使得 a * b ≥ n。

根据上面的推论我们可以得到结论：最大乘积的拆解方案中不包含 大于 3 的数。

所以最终 n = 3x + 2y，乘积 m = 3 ^ x + 2 ^ y

6 = 3 + 3 = 2 + 2 + 2, 显然 3 * 3 > 2 * 2 * 2, 所以任何3个2的都可以变成2个3。所以2出现的次数一定小于3次。

```python
class Solution(object):
    def integerBreak(self, n):
        if n == 2:
            return 1
        if n == 3:
            return 2
        
        ans = 1
        x = n / 3
        y = n % 3
        if y == 0:
            ans = 3 ** x
        if y == 1:
            ans = 3 ** (x-1) * 2 * 2
        if y == 2:
            ans = 3 ** x * 2
        return ans
```