---
title: "7 整数反转"
date: 2021-06-19T22:59:49+08:00
draft: false
tags: ["算法", "字符串", "反转"]
categories: ["技术"]
---
**题目**

给你一个 32 位的有符号整数 x ，返回将 x 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围 [−231,  231 − 1] ，就返回 0。

假设环境不允许存储 64 位整数（有符号或无符号）。
 

示例 1：
```
输入：x = 123
输出：321
```
示例 2：
```
输入：x = -123
输出：-321
```
示例 3：
```
输入：x = 120
输出：21
```

链接：https://leetcode-cn.com/problems/reverse-integer

**解题思路**

```python
class Solution(object):
    def reverse(self, x):
        y = 0
        flag = 1
        if x < 0:
            flag = -1
            x = -x
        while x > 0:
            if y > (2 ** 31 - 1)/10 or y < (-1 * 2 ** 31)/10:
                return 0
            y = y * 10 + x % 10
            x = x / 10
        y *= flag
        # 不能最终出来判断，因为不允许存储 64 位整数（有符号或无符号）。
        # if y > (2 ** 31 - 1) or y < (-1 * 2 ** 31): 
        #    return 0
        return y
```