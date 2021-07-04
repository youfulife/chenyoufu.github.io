---
title: "231 2 的幂"
date: 2021-07-04T06:58:30+08:00
draft: false
tags: ["算法", "数学", "位运算"]
categories: ["技术"]
---
**题目**

给你一个整数 n，请你判断该整数是否是 2 的幂次方。如果是，返回 true ；否则，返回 false 。

如果存在一个整数 x 使得 n == 2x ，则认为 n 是 2 的幂次方。

链接：https://leetcode-cn.com/problems/power-of-two

**解题思路**

**朴素循环判断**

* 时间复杂度：O(logn)
* 空间复杂度：O(1)

```python
class Solution(object):
    def isPowerOfTwo(self, n):
        if n <= 0:
            return False
        if n == 1:
            return True
        while n > 1:
            if n % 2 == 1:
                return False
            n = n / 2
        return True
```

优化

```java
class Solution {
    public boolean isPowerOfTwo(int n) {
        if (n <= 0) return false;
        while (n % 2 == 0) n /= 2;
        return n == 1;
    }
}
```

**位运算**

* n & n-1 可以将n最右边的1变为0
* 2的n次方，只能有1个位是1

```python
class Solution(object):
    def isPowerOfTwo(self, n):
        return  n > 0 and n & (n-1) == 0
```

**lowbit**

* n & (~n + 1) = n & -n 等于最低位1的值
* 如果一个数 nn 是 22 的幂，那么有 lowbit(n) = n 的性质（2 的幂的二进制表示中必然是最高位为 1，低位为 0）。

```python
class Solution(object):
    def isPowerOfTwo(self, n):
        return n > 0 and n & -n == n
```
