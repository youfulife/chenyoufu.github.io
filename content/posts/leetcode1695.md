---
title: "1695 删除子数组的最大得分"
date: 2021-07-20T23:08:53+08:00
draft: false
tags: ["算法", "滑动窗口"]
categories: ["技术"]
---

**题目**

给你一个正整数数组 nums ，请你从中删除一个含有 若干不同元素 的子数组。删除子数组的 得分 就是子数组各元素之 和 。

返回 只删除一个 子数组可获得的 最大得分 。

如果数组 b 是数组 a 的一个连续子序列，即如果它等于 a[l],a[l+1],...,a[r] ，那么它就是 a 的一个子数组。

示例 1：
```
输入：nums = [4,2,4,5,6]
输出：17
解释：最优子数组是 [2,4,5,6]
```
链接：https://leetcode-cn.com/problems/maximum-erasure-value

**解题思路**

跟 无重复字符的最长子串 是一个题，只不过是字符串换成了数组

```python
class Solution(object):
    def maximumUniqueSubarray(self, nums):
        left = 0
        right = 0
        window = {}
        ans = 0
        while right < len(nums):
            c = nums[right]
            right += 1
            window[c] = window.get(c, 0) + 1
            while window[c] > 1:
                d = nums[left]
                left += 1
                window[d] -= 1
            ans = max(ans, sum(nums[left:right]))
        return ans
```
