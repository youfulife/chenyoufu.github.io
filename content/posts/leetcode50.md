---
title: "50 Pow(x, n) 数值的整数次方"
date: 2021-06-06T11:46:31+08:00
draft: false
---
**题目**
实现 pow(x, n) ，即计算 x 的 n 次幂函数（即，xn）。

示例 1：
```
输入：x = 2.00000, n = 10
输出：1024.00000
```

链接：https://leetcode-cn.com/problems/powx-n

**解题思路**

**递归**

```python
class Solution(object):

    def quickMul(self, x, n):
        if n == 0:
            return 1
        ret = self.quickMul(x, n >> 1)
        ret *= ret
        if n & 0x1 == 1:
            ret *= x
        return ret

    def myPow(self, x, n):
        # x, n
        # 0, 0
        # 0, 5
        # 0, -1
        # 1, 0
        # 2, -5
        # 2, 3
        if x == 0:
            return 0
        
        ret = self.quickMul(x, abs(n))
        if n < 0:
            ret = 1.0/ret
        
        return ret
```

