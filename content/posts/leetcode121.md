---
title: "121 买卖股票的最佳时机"
date: 2021-06-15T23:15:25+08:00
draft: false
tags: ["算法", "数组", "股票问题"]
categories: ["技术"]
---

**题目**

给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。

链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock

**解题思路**

* 这个题本质就是要求某个数与其右边最大的数的差值
* 遍历价格数组一遍，记录历史最低点

```python
class Solution(object):
    def maxProfit(self, prices):
        minPrice = float('inf')
        maxP = 0
        for x in prices:
            if x < minPrice:
                minPrice = x
            else:
                maxP = max(maxP, x-minPrice)
        return maxP
```

----

2021.07.04 二刷

**单调栈**

* 在 pricesprices 数组的末尾加上一个 哨兵👨‍✈️(也就是一个很小的元素，这里设为 0))，就相当于作为股市收盘的标记(后面就清楚他的作用了)
* 假如栈空或者入栈元素大于栈顶元素，直接入栈
* 假如入栈元素小于栈顶元素则循环弹栈，直到入栈元素大于栈顶元素或者栈空
* 在每次弹出的时候，我们拿他与买入的值(也就是栈底)做差，维护一个最大值。

```python
class Solution(object):
    def maxProfit(self, prices):
        stack = []
        prices.append(0)
        ans = 0
        for x in prices:
            while stack and stack[-1] > x:
                ans = max(ans, stack[-1]-stack[0])
                stack.pop()
            stack.append(x)
        return ans
```

**动态规划**

* dp[i] = max(dp[i-1], price[i]-minPrice)
* 优化后就可以得到最上面第一种方法

```python
class Solution(object):
    def maxProfit(self, prices):
        # dp[i] = max(dp[i-1], price[i]-minPrice)
        minPrice = prices[0]
        n = len(prices)
        dp = [0] * n
        for i in range(1, n):
            minPrice = min(minPrice, prices[i])
            dp[i] = max(dp[i-1], prices[i]-minPrice)
        return max(dp)
```