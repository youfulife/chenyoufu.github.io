---
title: "1423 可获得的最大点数"
date: 2021-07-16T23:07:58+08:00
draft: false
tags: ["算法", "数组", "滑动窗口"]
categories: ["技术"]
---

**题目**

几张卡牌 排成一行，每张卡牌都有一个对应的点数。点数由整数数组 cardPoints 给出。

每次行动，你可以从行的开头或者末尾拿一张卡牌，最终你必须正好拿 k 张卡牌。

你的点数就是你拿到手中的所有卡牌的点数之和。

给你一个整数数组 cardPoints 和整数 k，请你返回可以获得的最大点数。

示例 1：
```
输入：cardPoints = [1,2,3,4,5,6,1], k = 3
输出：12
解释：第一次行动，不管拿哪张牌，你的点数总是 1 。但是，先拿最右边的卡牌将会最大化你的可获得点数。最优策略是拿右边的三张牌，最终点数为 1 + 6 + 5 = 12 。
```

链接：https://leetcode-cn.com/problems/maximum-points-you-can-obtain-from-cards

**解题思路**

* 想了10分钟没有想出来, 看了题解的思路

每一步拿牌都是从头或者尾拿, 所以最终剩余的牌一定是连续的, 所以本题换个思路:
从一个长度为n的数组中找出一个长度为n-k, 累加值最小的连续子序列[i, j], 那么剩余元素[0, i), (j, n)就是本题需要的解.

PS. 你会发现以下代码和 643. 子数组最大平均数 I 代码很相似，因为是一套模板，所以说这道其实是道简单题，只是多了一个小学奥数难度的等式推导过程 ~

初始化将滑动窗口压满，取得第一个滑动窗口的目标值

继续滑动窗口，每往前滑动一次，需要删除一个和添加一个元素

```python
class Solution(object):
    def maxScore(self, cardPoints, k):
        n = len(cardPoints)
        s = 0
        total = 0
        mins = 0
        for i in range(n-k):
            s += cardPoints[i]
        mins = s
        total = s
        for i in range(n-k, n):
            total += cardPoints[i]
            s = s + cardPoints[i] - cardPoints[i - (n-k)]
            mins = min(mins, s)
        return total-mins
```

**换一种思维，将数组看成是环形数组**

求 [n-k, n) .. [0, k) 这个范围内的k个数的最大的连续子序列和。

```python
class Solution(object):
    def maxScore(self, cardPoints, k):
        n = len(cardPoints)
        s = 0
        maxs = 0
        for i in range(n-k, n):
            s += cardPoints[i]
        maxs = s
        for i in range(0, k):
            s = s + cardPoints[i] - cardPoints[n-k+i]
            maxs = max(maxs, s)
        return maxs
```