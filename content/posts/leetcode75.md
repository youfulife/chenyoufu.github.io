---
title: "75 颜色分类"
date: 2021-07-15T18:44:42+08:00
draft: true
tags: ["算法", "数组", "快速排序"]
categories: ["技术"]
---

**题目**

给定一个包含红色、白色和蓝色，一共 n 个元素的数组，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

此题中，我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

示例 1：
```
输入：nums = [2,0,2,1,1,0]
输出：[0,0,1,1,2,2]
```

链接：https://leetcode-cn.com/problems/sort-colors

**解题思路**

**快速排序的分区算法，将数组分成3个区间**

* 第 1 部分严格小于 pivot 元素的值；
* 第 2 部分恰好等于 pivot 元素的值；
* 第 3 部分严格大于 pivot 元素的值。

```python
class Solution(object):
    def sortColors(self, nums):
        n = len(nums)
        # 定义循环不变量
        # all in [0, p0) == 0
        # all in [p0, i) == 1
        # all in (p2, len-1] == 2

        j = 0 # [0, j)
        k = n - 1 # (k, n-1]
        i = 0
        while i <= k:
            if nums[i] == 0:
                nums[j], nums[i] = nums[i], nums[j]
                j += 1
                i += 1
            elif nums[i] == 1:
                i += 1
            else:
                # 这里要注意i不能+1，因为可能是0，还要交换到前面的区间
                nums[i], nums[k] = nums[k], nums[i]
                k -= 1
        return nums
```


----

2021-11-27 二刷

想到用hash table，但是没记起来试用类似快速排序的partition思路。

```python
class Solution(object):
    def sortColors(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        m = {0:0, 1:0, 2:0}
        for x in nums:
            m[x] += 1
        
        for i in range(len(nums)):
            if i < m[0]:
                nums[i] = 0
            elif i < m[0] + m[1]:
                nums[i] = 1
            else:
                nums[i] = 2
        return  
```


partition思路

```python
class Solution(object):
    def sortColors(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        # [0, left), (right, n]
        n = len(nums)
        left = 0
        right = n
        i = 0
        while i < right:
            if nums[i] == 0:
                nums[left], nums[i] = nums[i], nums[left]
                left += 1
                i += 1
            elif nums[i] == 1:
                i += 1
            else:
                right -= 1
                nums[right], nums[i] = nums[i], nums[right]
                
        return  
```

