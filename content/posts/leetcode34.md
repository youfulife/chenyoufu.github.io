---
title: "34 在排序数组中查找元素的第一个和最后一个位置"
date: 2021-08-23T22:38:11+08:00
draft: false
tags: ["算法", "二分查找", "五星", "模版"]
categories: ["技术"]
---

**题目**

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

链接：https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array


**解题思路**

* 找到一个符合条件的值，左右可能还有符合条件的值，用标准的模版不好处理，但是可以把当前符合条件的解记录下来，然后根据条件调整区间
* 不需要乱七八糟的边界判断条件，跟找标准的值是几乎一摸一样的代码

```python
class Solution(object):
    def leftBound(self, nums, target):
        left = 0
        right = len(nums) - 1
        ans = -1
        while left <= right:
            mid = left + (right-left) / 2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid - 1
            else:
                # 先把存在解记录下来，然后调整区间，找下一个可能的值
                ans = mid
                right = mid - 1
        return ans

    def rightBound(self, nums, target):
        left = 0
        right = len(nums) - 1
        ans = -1
        while left <= right:
            mid = left + (right-left)/ 2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid - 1
            else:
                # 先把存在解记录下来，然后调整区间，找下一个可能的值
                ans = mid
                left = mid + 1
        return ans

    def searchRange(self, nums, target):
        l = self.leftBound(nums, target)
        r = self.rightBound(nums, target)
        return [l, r]
```