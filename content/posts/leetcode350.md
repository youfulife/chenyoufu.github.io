---
title: "350 两个数组的交集 II"
date: 2021-06-16T19:33:06+08:00
draft: false
tags: ["算法", "数组"]
categories: ["技术"]
---
**题目**

给定两个数组，编写一个函数来计算它们的交集。

示例 1：
```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]
```

示例 2:
```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[4,9]
```

说明：

* 输出结果中每个元素出现的次数，应与元素在两个数组中出现次数的最小值一致。
* 我们可以不考虑输出结果的顺序。

链接：https://leetcode-cn.com/problems/intersection-of-two-arrays-ii

**解题思路**

将第一个数组里的数放到一个map中，遍历第二个数组，存在的数加入到结果中，同时map中的count减1

```python
class Solution(object):
    def intersect(self, nums1, nums2):
        d = {}
        for x in nums1:
            d[x] = d.get(x, 0) + 1
        
        ret = []
        for x in nums2:
            if d.get(x, 0) > 0:
                d[x] -= 1
                ret.append(x)
        return ret
```

**排序**

如果已经排好序，可以用双指针。如果两个数字不相等，则将指向较小数字的指针右移一位，如果两个数字相等，将该数字添加到答案，并将两个指针都右移一位。当至少有一个指针超出数组范围时，遍历结束。

```python
class Solution(object):
    def intersect(self, nums1, nums2):
        nums1.sort()
        nums2.sort()
        p1 = 0
        p2 = 0
        ret = []
        while p1 < len(nums1) and p2 < len(nums2):
            if nums1[p1] < nums2[p2]:
                p1 += 1
            elif nums1[p1] > nums2[p2]:
                p2 += 1
            else:
                ret.append(nums1[p1])
                p1 += 1
                p2 += 1
        return ret
```