---
title: "1004 最大连续1的个数 III"
date: 2021-07-22T23:37:32+08:00
draft: false
tags: ["算法", "滑动窗口"]
categories: ["技术"]
---
**题目**
给定一个由若干 0 和 1 组成的数组 A，我们最多可以将 K 个值从 0 变成 1 。

返回仅包含 1 的最长（连续）子数组的长度。

示例 1：
```
输入：A = [1,1,1,0,0,0,1,1,1,1,0], K = 2
输出：6
解释： 
[1,1,1,0,0,1,1,1,1,1,1]
粗体数字从 0 翻转到 1，最长的子数组长度为 6。
```
链接：https://leetcode-cn.com/problems/max-consecutive-ones-iii

**解题思路**

跟之前的字母替换一样，只不过字母换成了0和1而已

```python
class Solution(object):
    def longestOnes(self, nums, k):
        left, right = 0, 0
        zeros = 0
        ans = 0
        while right < len(nums):
            c = nums[right]
            right += 1
            if c == 0:
                zeros += 1

            while zeros > k:
                d = nums[left]
                left += 1
                if d == 0:
                    zeros -= 1
            
            ans = max(ans, right-left)
        return ans
```