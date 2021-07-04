---
title: "198 打家劫舍"
date: 2021-07-04T08:53:15+08:00
draft: false
tags: ["算法", "数组", "动态规划"]
categories: ["技术"]
---

**题目**

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

示例 1：
```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

链接：https://leetcode-cn.com/problems/house-robber

**解题思路**

首先考虑最简单的情况。如果只有一间房屋，则偷窃该房屋，可以偷窃到最高总金额。如果只有两间房屋，则由于两间房屋相邻，不能同时偷窃，只能偷窃其中的一间房屋，因此选择其中金额较高的房屋进行偷窃，可以偷窃到最高总金额。

如果房屋数量大于两间，应该如何计算能够偷窃到的最高总金额呢？对于第 k~(k>2)k (k>2) 间房屋，有两个选项：

* 偷窃第 k 间房屋，那么就不能偷窃第 k-1 间房屋，偷窃总金额为前 k-2 间房屋的最高总金额与第 k 间房屋的金额之和。

* 不偷窃第 k 间房屋，偷窃总金额为前 k-1 间房屋的最高总金额。

在两个选项中选择偷窃总金额较大的选项，该选项对应的偷窃总金额即为前 k 间房屋能偷窃到的最高总金额。

```python
class Solution(object):
    def rob(self, nums):
        # dp[i] = max(dp[i-2]+nums[i], dp[i-1])
        # dp[0] = nums[0]
        # dp[1] = max(nums[0], nums[1])
        n = len(nums)
        if n == 0:
            return 0
        if n == 1:
            return nums[0]

        dp = [0] * n
        dp[0] = nums[0]
        dp[1] = max(nums[0], nums[1])
        for i in range(2, n):
            dp[i] = max(dp[i-2] + nums[i], dp[i-1])
        return dp[n-1]
```

**优化空间**

```python
class Solution(object):
    def rob(self, nums):
        n = len(nums)
        if n == 0:
            return 0
        if n == 1:
            return nums[0]

        f0 = nums[0]
        f1 = max(nums[0], nums[1])
        for i in range(2, n):
            f2 = max(f0 + nums[i], f1)
            f0 = f1
            f1 = f2

        return f1
```

**终极简洁**

```python
class Solution(object):
    def rob(self, nums):
        prev, cur = 0, 0
        for x in nums:
            prev, cur = cur, max(prev+x, cur)
        return cur
```