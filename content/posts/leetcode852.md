---
title: "852 山脉数组的峰顶索引"
date: 2021-06-19T17:37:30+08:00
draft: false
tags: ["算法", "数组", "二分查找"]
categories: ["技术"]
---
**数组**

符合下列属性的数组 arr 称为 山脉数组 ：
arr.length >= 3
存在 i（0 < i < arr.length - 1）使得：
arr[0] < arr[1] < ... arr[i-1] < arr[i]
arr[i] > arr[i+1] > ... > arr[arr.length - 1]
给你由整数组成的山脉数组 arr ，返回任何满足 arr[0] < arr[1] < ... arr[i - 1] < arr[i] > arr[i + 1] > ... > arr[arr.length - 1] 的下标 i 。

 

示例 1：
```
输入：arr = [0,1,0]
输出：1
```
示例 2：
```
输入：arr = [0,2,1,0]
输出：1
```

提示：

* 3 <= arr.length <= 104
* 0 <= arr[i] <= 106
* 题目数据保证 arr 是一个山脉数组


链接：https://leetcode-cn.com/problems/peak-index-in-a-mountain-array

**解题思路**

**循环判断 O(n)**

```python
class Solution(object):
    def peakIndexInMountainArray(self, arr):
        for i in range(len(arr)-1):
            if arr[i]> arr[i+1]:
                return i
```

**二分查找 O(log(n))**

```python
class Solution(object):
    def peakIndexInMountainArray(self, arr):
        left = 0
        right = len(arr)-1
        while left < right:
            mid = left + (right-left)/2
            if arr[mid] > arr[mid+1]:
                right = mid
            elif arr[mid] <= arr[mid+1]:
                left = mid + 1
        
        return left
```
