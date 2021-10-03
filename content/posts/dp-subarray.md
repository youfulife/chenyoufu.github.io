---
title: "动态规划 最大子数组和系列"
date: 2021-10-03T17:09:48+08:00
draft: false
tags: ["算法", "动态规划", "五星", "子数组和"]
categories: ["技术"]
---

### 题目

1. [53. 最大子序和](https://leetcode-cn.com/problems/maximum-subarray/)

给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

```python
class Solution(object):
    def maxSubArray(self, nums):
        n = len(nums)
        dp = [0] * n
        dp[0] = nums[0]
        for i in range(1, n):
            dp[i] = nums[i] + max(dp[i-1], 0)
        return max(dp)
```

2. [152. 乘积最大子数组](https://leetcode-cn.com/problems/maximum-product-subarray/)

给你一个整数数组 nums ，请你找出数组中乘积最大的连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

```python
class Solution(object):
    def maxProduct(self, nums):
        n = len(nums)
        dp1 = [1] * n
        dp2 = [1] * n
        dp1[0] = nums[0]
        dp2[0] = nums[0]
        
        for i in range(1, n):
            dp1[i] = max(nums[i], nums[i] * dp1[i-1], nums[i] * dp2[i-1])
            dp2[i] = min(nums[i], nums[i] * dp1[i-1], nums[i] * dp2[i-1])
        
        return max(dp1)
```

3. [918. 环形子数组的最大和](https://leetcode-cn.com/problems/maximum-sum-circular-subarray/)

给定一个由整数数组 A 表示的环形数组 C，求 C 的非空子数组的最大可能和。

```python
class Solution(object):
    def maxSubarraySumCircular(self, nums):
        if max(nums) < 0:
            return max(nums)

        n = len(nums)

        dp = [0] * n
        dpmin = [0] * n
        for i in range(n):
            dp[i] = nums[i] + max(0, dp[i-1])
            dpmin[i] = nums[i] + min(0, dpmin[i-1])

        return max(max(dp), sum(nums) - min(dpmin))
```