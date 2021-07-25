---
title: "795 区间子数组个数"
date: 2021-07-24T18:18:37+08:00
draft: false
tags: ["算法", "滑动窗口", "前缀和", "动态规划", "五星"]
categories: ["技术"]
---

**题目**

给定一个元素都是正整数的数组A ，正整数 L 以及 R (L <= R)。

求连续、非空且其中最大元素满足大于等于L 小于等于R的子数组个数。

```
输入: 
A = [2, 1, 4, 3]
L = 2
R = 3
输出: 3
解释: 满足条件的子数组: [2], [2, 1], [3].
```

链接：https://leetcode-cn.com/problems/number-of-subarrays-with-bounded-maximum

**解题思路**

滑动窗口 + 前缀和

```python
class Solution(object):
    def numSubarrayBoundedMax(self, nums, left, right):
        def lessEqualThan(nums, x):
            left, right = 0, 0
            ans = 0
            while right < len(nums):
                c = nums[right]
                right += 1
                if c > x:
                    left = right
                ans += right - left
            return ans
        return lessEqualThan(nums, right) - lessEqualThan(nums, left - 1)
```