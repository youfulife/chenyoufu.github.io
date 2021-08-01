---
title: "x的平方根"
date: 2021-07-27T22:23:33+08:00
draft: false
tags: ["算法", "二分搜索", "数学"]
categories: ["技术"]
---

**题目**

实现 int sqrt(int x) 函数。

计算并返回 x 的平方根，其中 x 是非负整数。

由于返回类型是整数，结果只保留整数的部分，小数部分将被舍去。

示例 1:
```
输入: 4
输出: 2
```
示例 2:
```
输入: 8
输出: 2
说明: 8 的平方根是 2.82842..., 
     由于返回类型是整数，小数部分将被舍去。
```

链接：https://leetcode-cn.com/problems/sqrtx

**解题思路**

**二分查找**

```python
class Solution(object):
    def mySqrt(self, x):
        left = 0
        right = x
        
        while True:
            mid = left + (right-left) / 2.0
            if int(mid * mid) > x:
                right = mid
            elif int(mid * mid) < x:
                left = mid
            else:
                return int(mid)
```
