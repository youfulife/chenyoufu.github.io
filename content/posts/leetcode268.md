---
title: "Leetcode268"
date: 2021-05-31T17:31:34+08:00
draft: false
tags: ["算法", "数组"]
categories: ["技术"]
---

**题目**

给定一个包含 [0, n] 中 n 个数的数组 nums ，找出 [0, n] 这个范围内没有出现在数组中的那个数。

进阶：

你能否实现线性时间复杂度、仅使用额外常数空间的算法解决此问题?

**示例 1：**
```
输入：nums = [3,0,1]
输出：2
解释：n = 3，因为有 3 个数字，所以所有的数字都在范围 [0,3] 内。2 是丢失的数字，因为它没有出现在 nums 中。
```
示例 2：
```
输入：nums = [0,1]
输出：2
解释：n = 2，因为有 2 个数字，所以所有的数字都在范围 [0,2] 内。2 是丢失的数字，因为它没有出现在 nums 中。
```

链接：https://leetcode-cn.com/problems/missing-number

**解题思路**

先用一个map来记录已经出现的数。

```python
class Solution(object):
    def missingNumber(self, nums):
        d = {}
        for n in nums:
            d[n] = 1
        
        for x in range(len(nums)): # 边界不能错
            if d.get(x, 0) == 0:
                return x
        
        return len(nums)
```

基于数字的和

```python
class Solution(object):
    def missingNumber(self, nums):
        n = len(nums)
        return (1 + n)* n/2 - sum(nums)
```


异或  1到n异或一遍，数组中的也异或一遍，相同的会是0，最后剩下的就是缺少的值。
```python
class Solution(object):
    def missingNumber(self, nums):
        res = len(nums) # 初始值设置为 n 很重要，这样就可以在一次循环中不需要特殊处理。
        for i, n in enumerate(nums):
            res ^= i
            res ^= n
        return res
```

