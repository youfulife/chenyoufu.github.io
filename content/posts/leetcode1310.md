---
title: "1310 子数组异或查询"
date: 2021-05-12T19:35:05+08:00
draft: false
tags: ["算法", "数组"]
categories: ["技术"]
---


有一个正整数数组 arr，现给你一个对应的查询数组 queries，其中 queries[i] = [Li, Ri]。

对于每个查询 i，请你计算从 Li 到 Ri 的 XOR 值（即 arr[Li] xor arr[Li+1] xor ... xor arr[Ri]）作为本次查询的结果。

并返回一个包含给定查询 queries 所有结果的数组。

示例 1：
```
输入：arr = [1,3,4,8], queries = [[0,1],[1,2],[0,3],[3,3]]
输出：[2,7,14,8] 
解释：
数组中元素的二进制表示形式是：
1 = 0001 
3 = 0011 
4 = 0100 
8 = 1000 
查询的 XOR 值为：
[0,1] = 1 xor 3 = 2 
[1,2] = 3 xor 4 = 7 
[0,3] = 1 xor 3 xor 4 xor 8 = 14 
[3,3] = 8
```

提示：

* 1 <= arr.length <= 3 * 10^4
* 1 <= arr[i] <= 10^9
* 1 <= queries.length <= 3 * 10^4
* queries[i].length == 2
* 0 <= queries[i][0] <= queries[i][1] < arr.length

链接：https://leetcode-cn.com/problems/xor-queries-of-a-subarray


**解题思路**

**暴力循环(超时) 时间 O(m*n)**

```python
class Solution(object):
    def xorQueries(self, arr, queries):
        ans = []
        for q in queries:
            l = q[0]
            r = q[1]
            ans.append(reduce(lambda x, y: x ^ y, arr[l:r+1]))
        return ans
```

**优化思路**


* a[i] ^ a[i+1] ^ ... ^ a[n] = (a[0] ^ a[1] ^ ... ^ a[i-1]) ^ (a[0] ^ a[1] ^ ... ^ a[n])
* xors[i] = a[0] ^ a[1] ^ ... ^ a[i-1]
* 其中 xors[0] = 0, len(xors) = len(arr) + 1
* 时间 O(n)

```python
class Solution(object):
    def xorQueries(self, arr, queries):
        xors = [0]
        for n in arr:
            xors.append(xors[-1] ^ n)
        
        ans = []
        for q in queries:
            l = q[0]
            r = q[1]
            ans.append(xors[l] ^ xors[r+1])
        return ans
```