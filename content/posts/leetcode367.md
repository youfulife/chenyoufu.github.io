---
title: "367 有效的完全平方数"
date: 2021-08-19T23:10:01+08:00
draft: flase
tags: ["算法", "二分查找"]
categories: ["技术"]
---

**题目**

给定一个 正整数 num ，编写一个函数，如果 num 是一个完全平方数，则返回 true ，否则返回 false 。

进阶：不要 使用任何内置的库函数，如  sqrt 。

示例 1：
```
输入：num = 16
输出：true
```

链接：https://leetcode-cn.com/problems/valid-perfect-square

**解题思路**

二分法 OlogN

```python
class Solution(object):
    def isPerfectSquare(self, num):
        left = 0
        right = num
        while left <= right:
            mid = left + (right - left )/2

            if mid * mid == num:
                return True
            elif mid * mid > num:
                right = mid - 1
            elif mid * mid < num:
                left = mid + 1
        return False
```