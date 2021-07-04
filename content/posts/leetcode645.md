---
title: "645 错误的集合"
date: 2021-07-04T21:23:48+08:00
draft: false
tags: ["算法", "数组"]
categories: ["技术"]
---
**题目**

集合 s 包含从 1 到 n 的整数。不幸的是，因为数据错误，导致集合里面某一个数字复制了成了集合里面的另外一个数字的值，导致集合 丢失了一个数字 并且 有一个数字重复 。

给定一个数组 nums 代表了集合 S 发生错误后的结果。

请你找出重复出现的整数，再找到丢失的整数，将它们以数组的形式返回。

示例 1：
```
输入：nums = [1,2,2,4]
输出：[2,3]
```

链接：https://leetcode-cn.com/problems/set-mismatch

**解体思路**

这个题目有很多不同的解法

* hash表存储每个数字出现的次数，然后查询[1, n]中每个元素出现的次数
* 值的范围在 [1, n]，所以可以将数组本身作为hash表
* 桶排序
* 数学求和 sum([1, n]), sum(nums), sum(set(nums)), 基于公式求解

```python
class Solution(object):
    def findErrorNums(self, nums):
        ans = []
        for i in range(len(nums)):
            x = abs(nums[i]) - 1
            if nums[x] < 0:
                ans.append(x+1)
            else:
                nums[x] *= -1
        for i in range(len(nums)):
            if nums[i] > 0:
                ans.append(i+1)
        return ans
```

