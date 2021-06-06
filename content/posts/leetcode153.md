---
title: "153 寻找旋转排序数组中的最小值"
date: 2021-06-05T14:24:10+08:00
draft: false
tags: ["算法", "数组", "二分查找"]
categories: ["技术"]
---

已知一个长度为 n 的数组，预先按照升序排列，经由 1 到 n 次 旋转 后，得到输入数组。例如，原数组 nums = [0,1,2,4,5,6,7] 在变化后可能得到：

* 若旋转 4 次，则可以得到 [4,5,6,7,0,1,2]
* 若旋转 7 次，则可以得到 [0,1,2,4,5,6,7]

注意，数组 [a[0], a[1], a[2], ..., a[n-1]] 旋转一次 的结果为数组 [a[n-1], a[0], a[1], a[2], ..., a[n-2]] 。

给你一个元素值 互不相同 的数组 nums ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 最小元素 。

示例 1：
```
输入：nums = [3,4,5,1,2]
输出：1
解释：原数组为 [1,2,3,4,5] ，旋转 3 次得到输入数组。
```

链接：https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array

**解题思路**

**线性搜索**

各种边界一定要考虑清楚：

* 旋转0次
* 旋转n次
* 空数组
* 只有一个元素的数组

```python
class Solution(object):
    def findMin(self, nums):
        if not nums:
            return None
        for i in range(len(nums)-1):
            if nums[i] > nums[i+1]:
                return nums[i+1]
        return nums[0]
```

**优化 二分查找**

```python
class Solution(object):
    def findMin(self, nums):
        if not nums:
            return None
        left = 0
        right = len(nums)-1
        mid = left
        while nums[left] > nums[right]:
            if right-left == 1:
                mid = right
                break
            mid = left + (right-left)/2
            if nums[mid] > nums[left]:
                left = mid
            elif nums[mid] < nums[right]:
                right = mid 
        return nums[mid]
```

**更加精简的版本**

用二分法查找，需要始终将目标值（这里是最小值）套住，并不断收缩左边界或右边界。

```python
class Solution(object):
    def findMin(self, nums):
        # [1]
        # []
        # [1,2,3]
        # [2,3,1]
        # [5,1,2,3,4]
        if not nums:
            return None
        left = 0
        right = len(nums)-1
        while left < right: # 退出循环 left == right
            mid = left + (right-left)/2
            if nums[mid] > nums[right]: # mid肯定不会是目标值，所以可以跨过
                left = mid + 1
            else:
                right = mid # mid 可能是目标值，所以right 要指向mid
        return nums[right]
```

