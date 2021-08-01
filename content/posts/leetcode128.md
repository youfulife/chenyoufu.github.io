---
title: "128 最长连续序列"
date: 2021-07-28T22:28:04+08:00
draft: false
tags: ["算法", "哈希表"]
categories: ["技术"]
---

给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 O(n) 的算法解决此问题。

示例 1：
```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```
示例 2：
```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

链接：https://leetcode-cn.com/problems/longest-consecutive-sequence

**解题思路**

第一次遇到，完全没思路

* 既然是要求O(N)， 排序肯定不行
* 数组中的每个数 x，考虑以其为起点，不断尝试匹配 x+1, x+2, ⋯ 是否存在
* 用一个哈希表存储数组中的数
* 假设 x 为一个连续序列的左边界，则 x - 1 就不可能存在于数组中。 因为，若 x - 1 存在于 nums 数组中，则 x 不可能成为连续序列的左边界。



```python
class Solution(object):
    def longestConsecutive(self, nums):
        m = {x: 1 for x in nums}
        ans = 0
        for x in nums:
            y = x
            l = 0
            while y in m and x-1 not in m:
                l += 1
                y += 1
            ans = max(ans, l)
        return ans
```

优化下代码

```python
def longestConsecutive(self, nums):
    nums = set(nums)
    best = 0
    for x in nums:
        if x - 1 not in nums:
            y = x + 1
            while y in nums:
                y += 1
            best = max(best, y - x)
    return best
```