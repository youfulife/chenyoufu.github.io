---
title: "53 最大子序和"
date: 2021-06-08T22:11:40+08:00
draft: false
tags: ["算法", "数组", "动态规划"]
categories: ["技术"]
---
**题目**
给定一个整数数组 nums ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
 
示例 1：
```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```
示例 2：
```
输入：nums = [1]
输出：1
```

链接：https://leetcode-cn.com/problems/maximum-subarray

**解题思路**

* dp[i] 表示以nums[i]为结尾的最大的子序和
* dp[i] = max(dp[i-1] + nums[i], nums[i])

```python
 class Solution(object):
    def maxSubArray(self, nums):
        dp = [min(nums)] * len(nums) 
        
        dp[0] = nums[0]

        for i in range(1, len(nums)):
            dp[i] = max(dp[i-1] + nums[i], nums[i])
        
        return max(dp)
```