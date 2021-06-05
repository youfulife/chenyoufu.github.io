---
title: "33 搜索旋转排序数组"
date: 2021-06-05T15:15:59+08:00
draft: false
tags: ["算法", "数组", "二分查找"]
categories: ["技术"]
---

**题目**

整数数组 nums 按升序排列，数组中的值 互不相同 。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。

例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。

给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。

```
输入：nums = [4,5,6,7,0,1,2], target = 0
输出：4
```
示例 2：
```
输入：nums = [4,5,6,7,0,1,2], target = 3
输出：-1
```

链接：https://leetcode-cn.com/problems/search-in-rotated-sorted-array

**解题思路**

找到旋转点，针对两侧数组分别进行二分查找。

```python
class Solution(object):
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        point = 0
        for i in range(len(nums)):
            if i > 0 and nums[i] < nums[i-1]:
                point = i
                break

        left = 0
        right = point-1
        while left <= right:
            mid = left + (right-left)/2
            if nums[mid] < target:
                left = mid+1
            elif nums[mid] > target:
                right = mid-1
            elif nums[mid] == target:
                return mid
        
        left = point
        right = len(nums) - 1
        while left <= right:
            mid = left + (right-left)/2
            if nums[mid] < target:
                left = mid+1
            elif nums[mid] > target:
                right = mid-1
            elif nums[mid] == target:
                return mid
        return -1
```

代码优化

```python

```