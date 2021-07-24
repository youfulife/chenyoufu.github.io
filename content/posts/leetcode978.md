---
title: "978 最长湍流子数组"
date: 2021-07-23T23:09:17+08:00
draft: false
tags: ["算法", "滑动窗口", "动态规划", "五星"]
categories: ["技术"]
---

**题目**

当 A 的子数组 A[i], A[i+1], ..., A[j] 满足下列条件时，我们称其为湍流子数组：

若 i <= k < j，当 k 为奇数时， A[k] > A[k+1]，且当 k 为偶数时，A[k] < A[k+1]；
或 若 i <= k < j，当 k 为偶数时，A[k] > A[k+1] ，且当 k 为奇数时， A[k] < A[k+1]。
也就是说，如果比较符号在子数组中的每个相邻元素对之间翻转，则该子数组是湍流子数组。

返回 A 的最大湍流子数组的长度。

示例 1：
```
输入：[9,4,2,10,7,8,8,1,9]
输出：5
解释：(A[1] > A[2] < A[3] > A[4] < A[5])
```
示例 2：
```
输入：[4,8,12,16]
输出：2
```

链接：https://leetcode-cn.com/problems/longest-turbulent-subarray

**解题思路**

有些费劲，自己没有想出来

**滑动窗口**

```python
class Solution(object):
    def maxTurbulenceSize(self, arr):
        # [left, right] 闭区间
        left, right = 0, 0
        ans = 1
        while right < len(arr) - 1:
            # 窗口开始
            if left == right:
                if arr[right] == arr[right + 1]:
                    left += 1
                right += 1
            else:
                # 满足条件
                if arr[right-1] > arr[right] and arr[right] < arr[right+1]:
                    right += 1
                # 满足条件
                elif arr[right-1] < arr[right] and arr[right] > arr[right+1]:
                    right += 1
                else: # 不满足
                    left = right
            ans = max(ans, right - left + 1)
        return ans
```

**动态规划**

todo