---
title: "326 3的幂"
date: 2021-07-04T07:55:31+08:00
draft: false
tags: ["算法", "数学", "位运算"]
categories: ["技术"]
---

**题目**

给定一个整数，写一个函数来判断它是否是 3 的幂次方。如果是，返回 true ；否则，返回 false 。

整数 n 是 3 的幂次方需满足：存在整数 x 使得 n == 3x

链接：https://leetcode-cn.com/problems/power-of-three

**解题思路**

**循环迭代**

* 找出数字 n 是否是数字 b 的幂的一个简单方法是，n%b 只要余数为 0，就一直将 n 除以 b

```python
class Solution(object):
    def isPowerOfThree(self, n):
        if n <= 0:
            return False
        while n % 3 == 0:
            n = n / 3
        return n == 1
```

