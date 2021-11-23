---
title: "动态规划 最长子序列系列"
date: 2021-10-02T22:52:09+08:00
draft: false
tags: ["算法", "动态规划", "套路模版", "五星", "子序列"]
categories: ["技术"]
---

### 总结

https://leetcode-cn.com/leetbook/read/dynamic-programming-1-plus/5o8l2i/

单串 dp[i] 线性动态规划最简单的一类问题，输入是一个串，状态一般定义为 dp[i] := 考虑[0..i]上，原问题的解，其中 i 位置的处理，根据不同的问题，主要有两种方式：

* 第一种是 i 位置必须取，此时状态可以进一步描述为 dp[i] := 考虑[0..i]上，且取 i，原问题的解；
* 第二种是 i 位置可以取可以不取

#### 状态的推导方向以及推导公式

1. 依赖比 i 小的 O(1) 个子问题

例如，当 f(dp[i-1], ...) = dp[i-1] + nums[i] 时，当前状态 dp[i] 仅与 dp[i-1] 有关。时间复杂度 O(n)，空间复杂度 O(n) 可以优化为 O(1)。

2. 依赖比 i 小的 O(n) 个子问题

dp[n] 与此前的更小规模的所有子问题 dp[n - 1], dp[n - 2], ..., dp[1] 都可能有关系。

其中 f 常见的有 max/min，可能还会对 i-1,i-2,...,0 有一些筛选条件，但推导 dp[n] 时依然是 O(n) 级的子问题数量。

以 min 函数为例，这种形式的问题的代码常见写法如下:

```python
for i = 1, ..., n
    for j = 1, ..., i-1
        dp[i] = min(dp[i], f(dp[j])
```

时间复杂度 O(n^2),空间复杂度 O(n)
### 题目

[674. 最长连续递增序列](https://leetcode-cn.com/problems/longest-continuous-increasing-subsequence/)

[300. 最长递增子序列](https://leetcode-cn.com/problems/longest-increasing-subsequence/)

[673. 最长递增子序列的个数](https://leetcode-cn.com/problems/number-of-longest-increasing-subsequence/)

[354. 俄罗斯套娃信封问题](https://leetcode-cn.com/problems/russian-doll-envelopes/)

[873. 最长的斐波那契子序列的长度](https://leetcode-cn.com/problems/length-of-longest-fibonacci-subsequence/)

[1027. 最长等差数列](https://leetcode-cn.com/problems/longest-arithmetic-subsequence/)

#### 674. 最长连续递增序列

**题目**

给定一个未经排序的整数数组，找到最长且 连续递增的子序列，并返回该序列的长度。

**解题思路**

```python
class Solution(object):
    def findLengthOfLCIS(self, nums):
        n = len(nums)
        dp = [1] * n

        for i in range(1, n):
            if nums[i] > nums[i-1]:
                dp[i] = dp[i-1] + 1
        
        return max(dp)
```
#### 300 最长递增子序列

**题目**

给你一个整数数组 nums ，找到其中最长严格递增子序列的长度。

子序列是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，[3,6,2,7] 是数组 [0,3,1,6,2,2,7] 的子序列。

**解题思路**

```python
class Solution(object):
    def lengthOfLIS(self, nums):
        n = len(nums)
        dp = [1] * n

        for i in range(n):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)

        return max(dp)
```

#### 600 最长递增子序列的个数

**题目**

给定一个未排序的整数数组，找到最长递增子序列的个数。

示例 1:
```
输入: [1,3,5,4,7]
输出: 2
解释: 有两个最长递增子序列，分别是 [1, 3, 4, 7] 和[1, 3, 5, 7]。
```

**解题思路**

```python
class Solution(object):
    def findNumberOfLIS(self, nums):
        n = len(nums)
        dp = [1] * n
        cnt = [1] * n
        
        for i in range(n):
            for j in range(i):
                if nums[i] > nums[j]:      
                    if dp[j] + 1 > dp[i]:
                        dp[i] = dp[j] + 1
                        cnt[i] = cnt[j]
                    elif dp[j] + 1 == dp[i]:
                        cnt[i] += cnt[j]
        maxS = max(dp)
        ans = 0

        for i in range(n):
            if dp[i] == maxS:
                ans += cnt[i] 
                    
        return ans
```

#### 354 俄罗斯套娃信封问题

**题目**

给你一个二维整数数组 envelopes ，其中 envelopes[i] = [wi, hi] ，表示第 i 个信封的宽度和高度。

当另一个信封的宽度和高度都比这个信封大的时候，这个信封就可以放进另一个信封里，如同俄罗斯套娃一样。

请计算 最多能有多少个 信封能组成一组“俄罗斯套娃”信封（即可以把一个信封放到另一个信封里面）。

注意：不允许旋转信封。

 
示例 1：
```
输入：envelopes = [[5,4],[6,4],[6,7],[2,3]]
输出：3
解释：最多信封的个数为 3, 组合为: [2,3] => [5,4] => [6,7]。
```
示例 2：
```
输入：envelopes = [[1,1],[1,1],[1,1]]
输出：1
```

**解题思路**

* 首先我们将所有的信封按照 w 值第一关键字升序、h 值第二关键字降序进行排序；
* 随后我们就可以忽略 w 维度，求出 h 维度的最长严格递增子序列，其长度即为答案。

```python
class Solution(object):
    def maxEnvelopes(self, envelopes):
        envelopes = sorted(envelopes, key=lambda x: (x[0], -x[1]))
        nums = [x[1] for x in envelopes]

        n = len(nums)
        dp = [1] * n
        
        for i in range(n):
            for j in range(i):
                if nums[i] > nums[j]:
                    dp[i] = max(dp[i], dp[j] + 1)
        
        return max(dp)
```

#### 873. 最长的斐波那契子序列的长度

**题目**

如果序列 X_1, X_2, ..., X_n 满足下列条件，就说它是 斐波那契式 的：

n >= 3
对于所有 i + 2 <= n，都有 X_i + X_{i+1} = X_{i+2}
给定一个严格递增的正整数数组形成序列 arr ，找到 arr 中最长的斐波那契式的子序列的长度。如果一个不存在，返回  0 。

（回想一下，子序列是从原序列 arr 中派生出来的，它从 arr 中删掉任意数量的元素（也可以不删），而不改变其余元素的顺序。例如， [3, 5, 8] 是 [3, 4, 5, 6, 7, 8] 的一个子序列）

**解题思路**

穷举

```python
class Solution(object):
    def lenLongestFibSubseq(self, arr):
        n = len(arr)
        s = set(arr)
        ans = 0
        for i in range(n):
            for j in range(i+1, n):
                a, b, l = arr[i], arr[j], 2
                while a + b in s:
                    a, b, l = b, a + b, l + 1
                    ans = max(ans, l)
        return ans
```

动态规划

* dp[a, b] represents the length of fibo sequence ends up with (a, b)
* Then we have dp[a, b] = (dp[b - a, a] + 1 ) or 2

```python
class Solution(object):
    def lenLongestFibSubseq(self, arr):
        n = len(arr)
        s = set(arr)
        dp = {}
        
        for i in range(n):
            for j in range(i):
                a, b = arr[j], arr[i]
                if b-a < a and b-a in s:
                    dp[(a, b)] = dp.get((b-a, a), 2) + 1
               
        return max(dp.values() or [0])
```

#### 1027. 最长等差数列

**题目**

给定一个整数数组 A，返回 A 中最长等差子序列的长度。

**解题思路**

* dp[index][diff] equals to the length of arithmetic sequence at index with difference diff.

```python
class Solution(object):
    def longestArithSeqLength(self, nums):
        n = len(nums)
        dp = {}
        for i in range(n):
            for j in range(i):
                diff = nums[i] - nums[j]
                dp[(i, diff)] =  dp.get((j, diff), 1) + 1
        return max(dp.values())
```

