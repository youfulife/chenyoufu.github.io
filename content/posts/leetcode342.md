---
title: "342 4的幂"
date: 2021-07-04T07:44:55+08:00
draft: false
tags: ["算法", "数学", "位运算"]
categories: ["技术"]
---

**题目**

给定一个整数，写一个函数来判断它是否是 4 的幂次方。如果是，返回 true ；否则，返回 false 。

整数 n 是 4 的幂次方需满足：存在整数 x 使得 n == 4x

链接：https://leetcode-cn.com/problems/power-of-four

**解题斯理**

* 4的幂首先是2的幂，所以二进制表示只能有一个1
* 判断二进制表示的1，是否满足4的幂的要求

```python
class Solution(object):
    def isPowerOfFour(self, n):
        if n <= 0:
            return False
        if n & (n-1) > 0:
            return False
        if 0x55555555 | n != 0x55555555:
            return False
        return True
```

* 其他类似写法

```java
 public boolean isPowerOfFour(int n) {
        return n>0 && (n & (n-1))==0 && (n&(0X55555555))==n;
    }
```

```java
  public boolean isPowerOfFour(int n) {
        return n>0 && (n&(n-1))==0 && (n&(0Xaaaaaaaa))==0;
    }
```

* 可以通过 nn 除以 3 的余数是否为 1 来判断 n 是否是 4 的幂

```python
class Solution:
    def isPowerOfFour(self, n: int) -> bool:
        return n > 0 and (n & (n - 1)) == 0 and n % 3 == 1
```

