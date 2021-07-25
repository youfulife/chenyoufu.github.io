---
title: "995 K 连续位的最小翻转次数"
date: 2021-07-24T15:51:54+08:00
draft: false
tags: ["算法", "滑动窗口" , "五星", "差分数组"]
categories: ["技术"]
---

**题目**

在仅包含 0 和 1 的数组 A 中，一次 K 位翻转包括选择一个长度为 K 的（连续）子数组，同时将子数组中的每个 0 更改为 1，而每个 1 更改为 0。

返回所需的 K 位翻转的最小次数，以便数组没有值为 0 的元素。如果不可能，返回 -1。 

示例 1：
```
输入：A = [0,1,0], K = 1
输出：2
解释：先翻转 A[0]，然后翻转 A[2]。
```

链接：https://leetcode-cn.com/problems/minimum-number-of-k-consecutive-bit-flips

**解题思路**

由于对同一个子数组执行两次翻转操作不会改变该子数组，所以对每个长度为 kk 的子数组，应至多执行一次翻转操作。

对于若干个 k 位翻转操作，改变先后顺序并不影响最终翻转的结果。不妨从 nums[0] 开始考虑，若 nums[0]=0，则必定要翻转从位置 0 开始的子数组；若 nums[0]=1，则不翻转从位置 0 开始的子数组。

按照这一策略，我们从左到右地执行这些翻转操作。由于翻转操作是唯一的，若最终数组元素均为 1，则执行的翻转次数就是最小的。

**滑动窗口**

* 参考 https://leetcode-cn.com/problems/minimum-number-of-k-consecutive-bit-flips/solution/hua-dong-chuang-kou-shi-ben-ti-zui-rong-z403l/

```python
class Solution(object):
    def minKBitFlips(self, nums, k):
        q = deque([])
        ans = 0
        for i in range(len(nums)):
            if q and i >= q[0] + k:
                q.popleft()
            if len(q) % 2 == nums[i]:
                if i + k > len(nums):
                    return -1 
                q.append(i)
                ans += 1

        return ans
```

**差分数组**

todo