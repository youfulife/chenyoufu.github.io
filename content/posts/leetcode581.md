---
title: "581 最短无序连续子数组"
date: 2021-08-04T22:30:01+08:00
draft: false
tags: ["算法", "双指针", "五星"]
categories: ["技术"]
---

**题目**

给你一个整数数组 nums ，你需要找出一个 连续子数组 ，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

请你找出符合题意的 最短 子数组，并输出它的长度。

示例 1：
```
输入：nums = [2,6,4,8,10,9,15]
输出：5
解释：你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
```
示例 2：
```
输入：nums = [1,2,3,4]
输出：0
```
示例 3：
```
输入：nums = [1]
输出：0
```

链接：https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray

**解题思路**

**双指针 + 排序**

```python
class Solution(object):
    def findUnsortedSubarray(self, nums):
        s = sorted(nums)
        left, right = 0, len(nums)-1
        while left <= right and nums[left] == s[left]:
            left += 1

        while left <= right and nums[right] == s[right]:
            right -= 1

        return right - left + 1
```

**双指针 + 线性扫描**

数组被分为 A, B, C 三段，且有
```
|--- A ---|----- B -----|--- C ---|
```
* A,C 都是升序序列。
* B 中任一元素均大于 A 中元素，且小于 C 中任一元素。

我们可以把问题转化为一下模型: 

分别求左右两个边界点, 使得右边界点右侧的所有元素值, 比边界内的最大值要大; 左边界点左侧的所有元素, 比边界内的最小值要小;

因此可以套用双指针的模型:

* 从左到右遍历, 只要碰到比已经遍历过路径内的最大值要小的元素, 则说明该元素需要被纳入到重排序的子数组中;
* 同理, 再从右往左遍历, 只要碰到比已经遍历过的路径内的最小值还要大的元素, 说明该元素也需要被纳入到重排序的子数组中;

```python
class Solution(object):
    def findUnsortedSubarray(self, nums):
        n = len(nums)
        # 左边界，从后往前的遍历，min标记最小的数，left记录上一次波动的位置
        left, minN = 0, nums[n-1]
        # 右边界，从前往后的遍历，max标记最大的数，right 记录上一次波动的位置
        right, maxN = -1, nums[0]
        for i in range(n):
            if nums[i] >= maxN:
                maxN = nums[i]
            else: # 不是递增，出现波动，更新边界
                right = i
            
            if nums[n-1-i] <= minN:
                minN = nums[n-1-i]
            else:
                left = n-1-i
        return right-left + 1
```