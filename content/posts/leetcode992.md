---
title: "992 K 个不同整数的子数组"
date: 2021-07-25T09:24:39+08:00
draft: false
tags: ["算法", "滑动窗口", "前缀和", "五星"]
categories: ["技术"]
---

**题目**

给定一个正整数数组 A，如果 A 的某个子数组中不同整数的个数恰好为 K，则称 A 的这个连续、不一定不同的子数组为好子数组。

（例如，[1,2,3,1,2] 中有 3 个不同的整数：1，2，以及 3。）

返回 A 中好子数组的数目。

示例 1：
```
输入：A = [1,2,1,2,3], K = 2
输出：7
解释：恰好由 2 个不同整数组成的子数组：[1,2], [2,1], [1,2], [2,3], [1,2,1], [2,1,2], [1,2,1,2].
```

链接：https://leetcode-cn.com/problems/subarrays-with-k-different-integers

**解题思路**

* 滑动窗口+前缀和
* 正好等于K的子数组个数 = 小于等于K的 - 小于等于K-1的
* 小于等于K的就是标准的滑动窗口模版题

```python
class Solution(object):
    def subarraysWithKDistinct(self, nums, k):

        def lessEqual(nums, k):
            left, right = 0, 0
            window = {}
            valid = 0
            ans = 0
            while right < len(nums):
                c = nums[right]
                right += 1
                window[c] = window.get(c, 0) + 1
                if window[c] == 1:
                    valid += 1                
                while valid > k:
                    d = nums[left]
                    left += 1
                    if window[d] == 1:
                        valid -= 1
                    window[d] -= 1
                ans += right - left
            return ans
        return lessEqual(nums, k) - lessEqual(nums, k-1)
```