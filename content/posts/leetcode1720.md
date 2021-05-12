---
title: "两道异或题"
date: 2021-05-06T22:27:32+08:00
draft: false
tags: ["算法", "数组"]
categories: ["技术"]
---

**1720 解码异或后的数组**

未知整数数组 arr 由 n 个非负整数组成。

经编码后变为长度为 n - 1 的另一个整数数组 encoded ，其中 encoded[i] = arr[i] XOR arr[i + 1] 。例如，arr = [1,0,2,1] 经编码后得到 encoded = [1,2,3] 。

给你编码后的数组 encoded 和原数组 arr 的第一个元素 first（arr[0]）。

请解码返回原数组 arr 。可以证明答案存在并且是唯一的。

示例 1：
```
输入：encoded = [1,2,3], first = 1
输出：[1,0,2,1]
解释：若 arr = [1,0,2,1] ，
那么 first = 1 且 encoded = [1 XOR 0, 0 XOR 2, 2 XOR 1] = [1,2,3]
```
示例 2：
```
输入：encoded = [6,2,7,3], first = 4
输出：[4,2,0,7,4]

```
提示：

* 2 <= n <= 104
* encoded.length == n - 1
* 0 <= encoded[i] <= 105
* 0 <= first <= 105

链接：https://leetcode-cn.com/problems/decode-xored-array

**解题思路**

a ^ b = c

b = c ^ a

自己写的代码

```python
class Solution(object):
    def decode(self, encoded, first):
        ret = [first]
        a = first
        for i in encoded:
            b = a ^ i
            ret.append(b)
            a = b
        return ret
```

看题解优化后的代码

```python
class Solution(object):
    def decode(self, encoded, first):
        ret = [first]
        for n in encoded:
            ret.append(ret[-1] ^ n)
        return ret
```

---------

2021.05.11

**1734  解码异或后的排列**

给你一个整数数组 perm ，它是前 n 个正整数的排列，且 n 是个 奇数 。

它被加密成另一个长度为 n - 1 的整数数组 encoded ，满足 encoded[i] = perm[i] XOR perm[i + 1] 。比方说，如果 perm = [1,3,2] ，那么 encoded = [2,1] 。

给你 encoded 数组，请你返回原始数组 perm 。题目保证答案存在且唯一。

 
示例 1：

```
输入：encoded = [3,1]
输出：[1,2,3]
解释：如果 perm = [1,2,3] ，那么 encoded = [1 XOR 2,2 XOR 3] = [3,1]
```

示例 2：

```
输入：encoded = [6,5,4,6]
输出：[2,4,1,5,3]
```

**提示：**

3 <= n < 105
n 是奇数。
encoded.length == n - 1

链接：https://leetcode-cn.com/problems/decode-xored-permutation

**解题思路**

这个题目和上一个的不同之处在于不知道first的值, 需要根据题目给出的限定条件首先算出来first的值。

我们可以使用A, B, C, D, E代表整数数组perm，注意：它是前 n 个正整数的排列，且 n 是奇数。

为了表达的方便，可以这么定义：将A XOR B（A和B进行异或运算）简写为AB。

思路步骤：

既然我们知道了perm = [A, B, C, D, E]，那么encoded = [AB, BC, CD, DE]；

根据perm，我们可以得到ABCDE,根据encoded的BC和DE，我们可以得到BCDE；

将ABCDE和BCDE进行异或运算，得到A，即perm的第一个元素。这时候，题目转换成上面的第一题。

```python
class Solution(object):
    def decode(self, encoded):
        x = 0
        for i in range(1, len(encoded)+2):
            x ^= i
        
        y = 0
        for i in range(1, len(encoded), 2):
            y ^= encoded[i]
        
        a1 = x ^ y
        perm = [a1]
        for n in encoded:
            perm.append(perm[-1] ^ n)

        return perm
```