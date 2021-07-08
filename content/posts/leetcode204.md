---
title: "204 计数质数"
date: 2021-07-06T11:29:23+08:00
draft: false
tags: ["算法", "数学", "简单I"]
categories: ["技术"]
---

**题目**

统计所有小于非负整数 n 的质数的数量。

示例 1：
```
输入：n = 10
输出：4
解释：小于 10 的质数一共有 4 个, 它们是 2, 3, 5, 7 。
```

链接：https://leetcode-cn.com/problems/count-primes

**解题思路**

**暴力遍历，时间超出限制**

```python
class Solution(object):
    def countPrimes(self, n):
        # 拿到题后自己的思考
        # 如何判断一个数是质数？
        # 对每一个数暴力除以每个数判断？感觉明显不行。
        # 肯定有个数学公式
        # 先暴力求解一波
        count = 0
        for i in range(2, n):
            if self.isPrime(i):
                count += 1
        return count
    
    def isPrime(self, n):
        if n < 2:
            return False
        if n == 2:
            return True
        for i in range(2, n/2+1):
            if n % i == 0:
                return False
        return True
```

**线性法**

```python
class Solution(object):
    def countPrimes(self, n):
        primes = []
        isPrimes = [1] * n
        for i in range(2, n):
            if isPrimes[i] == 1:
                primes.append(i)
            for prime in primes:
                if i * prime >= n:
                    break
                isPrimes[i * prime] = 0
                if i % prime == 0:
                    break
        return len(primes)
```