---
title: "29 两数相除"
date: 2021-07-03T21:36:39+08:00
draft: false
tags: ["算法", "数学", "二分查找"]
categories: ["技术"]
---

**题目**

给定两个整数，被除数 dividend 和除数 divisor。将两数相除，要求不使用乘法、除法和 mod 运算符。

返回被除数 dividend 除以除数 divisor 得到的商。

整数除法的结果应当截去（truncate）其小数部分，例如：truncate(8.345) = 8 以及 truncate(-2.7335) = -2

示例 1:
```
输入: dividend = 10, divisor = 3
输出: 3
解释: 10/3 = truncate(3.33333..) = truncate(3) = 3
```

链接：https://leetcode-cn.com/problems/divide-two-integers

**解题思路**

```python
class Solution(object):
    def divide(self, dividend, divisor):
        x = abs(divisor)
        reminder = abs(dividend)
        ans = 0
        while x <= reminder:
            count = 1
            div = x
            while div + div < reminder:
                div += div
                count += count
            reminder -= div
            ans += count
            
        if (dividend > 0 and divisor < 0) or (dividend < 0 and divisor > 0):
            ans = -ans
        if ans >= 2**31:
            ans = 2**31-1

        return ans
```



