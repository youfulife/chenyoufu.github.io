---
title: "1052 爱生气的书店老板"
date: 2021-07-15T22:40:43+08:00
draft: false
tags: ["算法", "数组", "滑动窗口", "好题"]
categories: ["技术"]
---

**题目**
今天，书店老板有一家店打算试营业 customers.length 分钟。每分钟都有一些顾客（customers[i]）会进入书店，所有这些顾客都会在那一分钟结束后离开。

在某些时候，书店老板会生气。 如果书店老板在第 i 分钟生气，那么 grumpy[i] = 1，否则 grumpy[i] = 0。 当书店老板生气时，那一分钟的顾客就会不满意，不生气则他们是满意的。

书店老板知道一个秘密技巧，能抑制自己的情绪，可以让自己连续 X 分钟不生气，但却只能使用一次。

请你返回这一天营业下来，最多有多少客户能够感到满意。
 
示例：
```
输入：customers = [1,0,1,2,1,1,7,5], grumpy = [0,1,0,1,0,1,0,1], X = 3
输出：16
解释：
书店老板在最后 3 分钟保持冷静。
感到满意的最大客户数量 = 1 + 1 + 1 + 1 + 7 + 5 = 16.
```

链接：https://leetcode-cn.com/problems/grumpy-bookstore-owner

**解题思路**

* 使用「秘密技巧」能得到的最终的顾客数 = 所有不生气时间内的顾客总数 + 在窗口 X 内使用「秘密技巧」挽留住的原本因为生气而被赶走顾客数。
* 计算所有不生气时间内的顾客总数。
* 在窗口 X 内因为生气而被赶走的顾客数：使用大小为 X 的滑动窗口，计算滑动窗口内的老板生气时对应的顾客数。

```python
class Solution(object):
    def maxSatisfied(self, customers, grumpy, minutes):
        n = len(grumpy)
        s = 0
        for i in range(n):
            s += customers[i] * (1 - grumpy[i])
        
        s1 = 0
        for i in range(minutes):
            s1 += customers[i] * grumpy[i]
        maxs1 = s1
        for i in range(minutes, len(grumpy)):
            s1 = s1 + customers[i] * grumpy[i] - customers[i-minutes] * grumpy[i-minutes]
            maxs1 = max(maxs1, s1)
        
        ans = s + maxs1
        return ans
```

**思路进一步优化**

由于「技巧」只会将情绪将「生气」变为「不生气」，不生气仍然是不生气。

我们可以先将原本就满意的客户加入答案，同时将对应的 customers[i] 变为 0。

之后的问题转化为：在 customers 中找到连续一段长度为 x 的子数组，使得其总和最大。这部分就是我们应用秘密技巧所得到的客户。

```python
class Solution(object):
    def maxSatisfied(self, customers, grumpy, minutes):
        n = len(grumpy)
        s = 0
        for i in range(n):
            if grumpy[i] == 0:
                s += customers[i]
                customers[i] = 0 # 这一步非常妙
        s1 = 0
        for i in range(minutes):
            s1 += customers[i]
        maxs = s + s1
        for i in range(minutes, n):
            s1 = s1 + customers[i] - customers[i-minutes]
            maxs = max(maxs, s + s1)
        return maxs
```
