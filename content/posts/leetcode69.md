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

2021.08.31 二刷

做了挺长时间又没做出来，边界老是报错，最终又是看了题解。

步骤：

1. 如果恰好找到一个数满足平方等于x，就返回这个数
2. 如果平方小于x，说明这个数可能是答案，因为答案是下取整的，先做一个标记，然后对这个数的右侧去进行二分查找，如果有更接近这个x的平方根，这个标记就会被更新。
3. 如果平方大于x，说明这个数不可能是答案，此时直接对该数的左侧进行二分查找。

```python
class Solution(object):
    def mySqrt(self, x):
        left, right = 0, x
        ans = -1
        
        while left <= right:
            mid = left + (right-left) / 2
            if mid * mid > x:
                right = mid-1
            elif mid * mid <= x:
                ans = mid
                left = mid + 1
        return ans
```