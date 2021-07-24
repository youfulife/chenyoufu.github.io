---
title: "1493 删掉一个元素以后全为 1 的最长子数组"
date: 2021-07-23T22:36:05+08:00
draft: false
tags: ["算法", "滑动窗口"]
categories: ["技术"]
---

**题目**

给你一个二进制数组 nums ，你需要从中删掉一个元素。

请你在删掉元素的结果数组中，返回最长的且只包含 1 的非空子数组的长度。

如果不存在这样的子数组，请返回 0 。

提示 1：
```
输入：nums = [1,1,0,1]
输出：3
解释：删掉位置 2 的数后，[1,1,1] 包含 3 个 1 。
```

链接：https://leetcode-cn.com/problems/longest-subarray-of-1s-after-deleting-one-element

**解题思路**

标准滑动窗口题目

* 0的个数作为窗口的收缩边界
* 保证窗口内0的个数不大于要删除的元素个数就行，这里为1
* 计算所有的可能取值，保留最大值即可

```python
class Solution(object):
    def longestSubarray(self, nums):
        left = 0
        right = 0
        zeros = 0
        ans = 0
        while right < len(nums):
            c = nums[right]
            right += 1
            if c == 0:
                zeros += 1
            
            while zeros > 1:
                d = nums[left]
                left += 1
                if d == 0:
                    zeros -= 1
            ans = max(ans, right - left - 1)
        return ans
```