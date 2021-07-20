---
title: "1658 将 x 减到 0 的最小操作数"
date: 2021-07-18T22:24:11+08:00
draft: false
tags: ["算法", "字符串", "滑动窗口"]
categories: ["技术"]
---
**题目**
给你一个整数数组 nums 和一个整数 x 。每一次操作时，你应当移除数组 nums 最左边或最右边的元素，然后从 x 中减去该元素的值。请注意，需要 修改 数组以供接下来的操作使用。

如果可以将 x 恰好 减到 0 ，返回 最小操作数 ；否则，返回 -1 。

提示：

* 1 <= nums.length <= 105
* 1 <= nums[i] <= 104
* 1 <= x <= 109

链接：https://leetcode-cn.com/problems/minimum-operations-to-reduce-x-to-zero

**解题思路** 

和最大点数的那个题目类似，可以换个角度来做题。

* 最短的连续子序列和为x，要从头或者尾巴开始
* 转化为求和为 sum(nums)-x 的最长连续子序列
* 用数组长度减去最长连续子序列，剩下的就是和为x的最短连续子序列
* 各种边界条件判断要注意，因为边界考虑不周提交了4次才通过

```python
class Solution(object):
    def minOperations(self, nums, x):
        total = sum(nums)
        target = total - x
        n = len(nums)

        left = 0
        right = 0
        s = 0
        # 3. 这里不能设置maxs=0, 否则会和target=0的情况冲突
        maxs = -1
        # 1. 没有加left < n 判断
        while left < n and right < n:
            s += nums[right]
            right += 1

            # 1. 没有加left < n 判断
            while left < n and s > target:
                s -= nums[left]
                left += 1
            
            if s == target:
                maxs = max(maxs, right-left)
        # 2. maxs >= 0 开始只写了 maxs > 0
        return n - maxs if maxs >= 0  else -1
```

上述边界条件如果根据题意提前判断，那么可以套用标准的滑动窗口模版

```python
class Solution(object):
    def minOperations(self, nums, x):
        n = len(nums)

        total = sum(nums)
        target = total - x
        # 提前判断
        if target < 0:
            return -1
        if target == 0:
            return n

        left = 0
        right = 0
        s = 0
        maxs = 0
        while right < n:
            s += nums[right]
            right += 1

            while s > target:
                s -= nums[left]
                left += 1
            
            if s == target:
                maxs = max(maxs, right-left)
        return n - maxs if maxs > 0 else -1
```