---
title: "338 比特位计数"
date: 2021-07-04T09:59:03+08:00
draft: false
tags: ["算法", "位运算", "动态规划"]
categories: ["技术"]
---
**题目**

给定一个非负整数 num。对于 0 ≤ i ≤ num 范围中的每个数字 i ，计算其二进制数中的 1 的数目并将它们作为数组返回。

示例 1:
```
输入: 2
输出: [0,1,1]
```
示例 2:
```
输入: 5
输出: [0,1,1,2,1,2]
```

链接：https://leetcode-cn.com/problems/counting-bits

**解题思路**

**朴素遍历**

```python
class Solution(object):
    def countBits(self, n):
        ans = []
        for i in range(n+1):
            ans.append(self.popcount(i))
        return ans

    def popcount(self, x):
        count = 0
        while x != 0:
            x = x & x-1
            count += 1
        return count
```

**动态规划 最低有效位**

* 奇数：二进制表示中，奇数一定比前面那个偶数多一个 1，因为多的就是最低位的 1
* 偶数：二进制表示中，偶数中 1 的个数一定和除以 2 之后的那个数一样多。因为最低位是 0，除以 2 就是右移一位，也就是把那个 0 抹掉而已，所以 1 的个数是不变的。

```python
class Solution(object):
    def countBits(self, n):
        # dp[i] = dp[i>>1] + (i & 1)
        dp = [0] * (n+1)
        for i in range(1, n+1):
            dp[i] = dp[i>>1] + (i & 1)
        return dp
```

**动态规划 最低设置位**

* 定义正整数 x 的「最低设置位」为 x 的二进制表示中的最低的 1 所在位。
* 令 y=x & (x−1)，则 y 为将 x 的最低设置位从 1 变成 0 之后的数，显然 bits[x]=bits[y]+1。

```python
class Solution(object):
    def countBits(self, n):
        # dp[i] = dp[i & i-1] + 1
        dp = [0] * (n+1)
        for i in range(1, n+1):
            dp[i] = dp[i & i-1] + 1
        return dp
```