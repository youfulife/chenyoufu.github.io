---
title: "88 合并两个有序数组"
date: 2021-06-03T22:06:22+08:00
draft: false
tags: ["算法", "数组", "双指针"]
categories: ["技术"]
---

**题目**

给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。你可以假设 nums1 的空间大小等于 m + n，这样它就有足够的空间保存来自 nums2 的元素。

示例 1：
```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
```
示例 2：
```
输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
```

链接：https://leetcode-cn.com/problems/merge-sorted-array

**解题思路**

逆向双指针，从尾巴开始比较，大的放入nums1的尾巴。

```python
class Solution(object):
    def merge(self, nums1, m, nums2, n):
    
        while m > 0 and n > 0:
            if nums1[m-1] > nums2[n-1]:
                nums1[m+n-1] = nums1[m-1]
                m -= 1
            else:
                nums1[m+n-1] = nums2[n-1]
                n -= 1
        while n > 0:
            nums1[m+n-1] = nums2[n-1]
            n -= 1
        return
```

-----

2021.07.03 二刷 思路记忆犹新