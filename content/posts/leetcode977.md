---
title: "977 有序数组的平方"
date: 2021-08-20T06:36:06+08:00
draft: false
tags: ["算法", "双指针", "五星", "逆向思维"]
categories: ["技术"]
---

**题目**

给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

示例：
```
输入：nums = [-7,-3,2,3,11]
输出：[4,9,9,49,121]
```

链接：https://leetcode-cn.com/problems/squares-of-a-sorted-array

**解题思路**

找到正负的分界点，然后把题目转化为两个有序数组的合并, 归并排序。

```python
class Solution(object):
    def sortedSquares(self, nums):
        n = len(nums)
        i = 0
        # 不要忘记判断 i<n, 因为可能全是负数
        while i < n and nums[i] < 0:
            i += 1
        # left = nums[:i]
        # right = nums[i:]

        ans = []
        # j = i 要在前面，否则i被修改掉，j再赋值会出错
        j = i
        i = i - 1
        # 合并两个有序数组
        while i >= 0 and j < n:
            a = nums[i] * nums[i]
            b = nums[j] * nums[j]
            if a < b:
                ans.append(a)
                i -= 1
            else:
                ans.append(b)
                j += 1
        while i >= 0:
            ans.append(nums[i] * nums[i])
            i -= 1
        while j < n:
            ans.append(nums[j] * nums[j])
            j += 1
        return ans
```

**逆向思维**

使用两个指针分别指向位置 00 和 n-1n−1，每次比较两个指针对应的数，选择较大的那个逆序放入答案并移动指针。这种方法无需处理某一指针移动至边界的情况，可以仔细思考其精髓所在。

```python
class Solution(object):
    def sortedSquares(self, nums):
        n = len(nums)
        i = 0
        j = n - 1
        ans = []
        while i <= j:
            a = nums[i] * nums[i]
            b = nums[j] * nums[j]
            if a < b:
                ans.append(b)
                j -= 1
            else:
                ans.append(a)
                i += 1
        
        return ans[::-1]
```

继续优化下上面的代码，不用队列或者反转链表

```python
class Solution(object):
    def sortedSquares(self, nums):
        n = len(nums)
        i = 0
        j = n - 1
        pos = n - 1
        ans = [0] * n
        while i <= j:
            a = nums[i]
            b = nums[j]
            if abs(a) < abs(b):
                ans[pos] = b * b
                j -= 1
            else:
                ans[pos] = a * a
                i += 1
            pos -= 1
        
        return ans
```