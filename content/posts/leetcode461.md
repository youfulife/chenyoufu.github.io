---
title: "461 汉明距离"
date: 2021-07-04T10:59:32+08:00
draft: false
tags: ["算法", "位运算"]
categories: ["技术"]
---

**题目**

两个整数之间的 汉明距离 指的是这两个数字对应二进制位不同的位置的数目。

给你两个整数 x 和 y，计算并返回它们之间的汉明距离。

示例 1：
```
输入：x = 1, y = 4
输出：2
解释：
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑
上面的箭头指出了对应二进制位不同的位置。
```

链接：https://leetcode-cn.com/problems/hamming-distance

**解题思路**

```python
class Solution(object):
    def hammingDistance(self, x, y):
        z = x ^ y
        count = 0
        while z > 0:
            z &= z-1
            count += 1
        return count
```