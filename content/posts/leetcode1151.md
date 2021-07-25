---
title: "1151 最少交换次数来组合所有的 1"
date: 2021-07-24T17:08:43+08:00
draft: false
tags: ["算法", "滑动窗口", "五星"]
categories: ["技术"]
---

**题目**

给出一个二进制数组 data，你需要通过交换位置，将数组中 任何位置 上的 1 组合到一起，并返回所有可能中所需 最少的交换次数。

示例 1：
```
输入：[1,0,1,0,1]
输出：1
解释： 
有三种可能的方法可以把所有的 1 组合在一起：
[1,1,1,0,0]，交换 1 次；
[0,1,1,1,0]，交换 2 次；
[0,0,1,1,1]，交换 1 次。
所以最少的交换次数为 1。
```

链接：https://leetcode-cn.com/problems/minimum-swaps-to-group-all-1s-together

**解题思路**

* 滑动窗口
* 统计所有的1的个数k
* 维持k长度的滑动窗口，统计窗口内0的个数，就是需要交换的次数

```python
class Solution(object):
    def minSwaps(self, data):
        k = sum(data)
        ans = 0
        zeros = 0
        for i in range(k):
            if data[i] == 0:
                zeros += 1
        ans = zeros
        for i in range(k, len(data)):
            zeros = zeros + (1 - data[i]) - (1-data[i-k])
            ans = min(ans, zeros)
        return ans
```

优化下代码的简洁性，统计窗口内1的个数，用 k - ones就是需要交换的次数

```python
class Solution(object):
    def minSwaps(self, data):
        k = sum(data)
        ans = 0
        ones = sum(data[:k])
        ans = k-ones
        for i in range(k, len(data)):
            ones = ones + data[i] - data[i-k]
            ans = min(ans, k - ones)
        return ans
```
