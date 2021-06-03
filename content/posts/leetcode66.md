---
title: "66 加一"
date: 2021-06-03T22:43:02+08:00
draft: false
tags: ["算法", "数组"]
categories: ["技术"]
---

**题目**

给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

示例 1：
```
输入：digits = [1,2,3]
输出：[1,2,4]
解释：输入数组表示数字 123。
```

链接：https://leetcode-cn.com/problems/plus-one

**解题思路**


```python
class Solution(object):
    def plusOne(self, digits):
        n = len(digits)
        flag = 0
        while n > 0:
            digits[n-1] += 1
            if digits[n-1] == 10:
                digits[n-1] = 0
                n -= 1
                flag = 1
            else:
                flag = 0
                break
        if digits[0] == 0 and flag == 1: # 标志位很重要
            digits[0] = 0
            digits = [1] + digits
        return digits
            
```