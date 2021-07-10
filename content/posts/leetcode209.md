---
title: "209 长度最小的子数组"
date: 2021-07-10T23:07:38+08:00
draft: false
tags: ["算法", "数组", "滑动窗口"]
categories: ["技术"]
---

**题目**

给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其和 ≥ target 的长度最小的 连续子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

示例 1：
```
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

链接：https://leetcode-cn.com/problems/minimum-size-subarray-sum

**解题思路**

滑动窗口

```python
class Solution(object):
    def minSubArrayLen(self, target, nums):
        left = 0
        right = 0
        sum = 0
        # ans的初始值不能设置为0
        ans = float('inf')
        while right < len(nums):
            sum += nums[right]
            right += 1

            while sum >= target:
                ans = min(ans, right - left)
                sum -= nums[left]
                left += 1
        # 边界判断
        if ans == float('inf'):
            ans = 0
        return ans
```