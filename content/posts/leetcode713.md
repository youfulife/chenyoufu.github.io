---
title: "713 乘积小于K的子数组"
date: 2021-07-25T09:59:00+08:00
draft: false
tags: ["算法", "滑动窗口", "五星"]
categories: ["技术"]
---

**题目**

给定一个正整数数组 nums和整数 k 。

请找出该数组内乘积小于 k 的连续的子数组的个数。

示例 1:
```
输入: nums = [10,5,2,6], k = 100
输出: 8
解释: 8个乘积小于100的子数组分别为: [10], [5], [2], [6], [10,5], [5,2], [2,6], [5,2,6]。
需要注意的是 [10,5,2] 并不是乘积小于100的子数组。
```
示例 2:
```
输入: nums = [1,2,3], k = 0
输出: 0
```

链接：https://leetcode-cn.com/problems/subarray-product-less-than-k

**解题思路**

**滑动窗口**

```python
class Solution(object):
    def numSubarrayProductLessThanK(self, nums, k):
        left, right = 0, 0
        mul = 1
        ans = 0
        while right < len(nums):
            c = nums[right]
            right += 1
            mul *= c

            # 这里不能忘记 left < right 这个条件
            while mul >= k and left < right:
                d = nums[left]
                left += 1
                mul /= d
            
            ans += right - left
        return ans
```