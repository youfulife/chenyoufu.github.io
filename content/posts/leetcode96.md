---
title: "96 不同的二叉搜索树"
date: 2021-09-28T23:15:26+08:00
draft: false
tags: ["算法", "数学", "动态规划"]
categories: ["技术"]
---

**题目**

给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？返回满足题意的二叉搜索树的种数。

**解题思路**

本题和整数拆分类似。

* dp[i] ： 1到i为节点组成的二叉搜索树的个数为dp[i]。
* 递推公式：dp[i] += dp[j - 1] * dp[i - j]; ，j-1 为j为头结点左子树节点数量，i-j 为以j为头结点右子树节点数量
* 确定遍历顺序, 首先一定是遍历节点数，从递归公式：dp[i] += dp[j - 1] * dp[i - j]可以看出，节点数为i的状态是依靠 i之前节点数的状态。

那么遍历i里面每一个数作为头结点的状态，用j来遍历。

```python
class Solution(object):
    def numTrees(self, n):
        dp = [0] * (n+1)
        dp[0] = 1

        for i in range(1, n+1):
            for j in range(1, i+1):
                dp[i] += dp[j-1] * dp[i-j]
        
        return dp[n]
```


**数学**

链接：https://leetcode-cn.com/problems/unique-binary-search-trees/solution/bu-tong-de-er-cha-sou-suo-shu-by-leetcode-solution/

卡特兰数

```python
class Solution(object):
    def numTrees(self, n):
        C = 1
        for i in range(0, n):
            C = C * 2*(2*i+1)/(i+2)
        return int(C)
```