---
title: "122 买卖股票的最佳时机 II"
date: 2021-06-15T22:58:34+08:00
draft: false
tags: ["算法", "数组", "股票问题"]
categories: ["技术"]
---

**题目**
给定一个数组 prices ，其中 prices[i] 是一支给定股票第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你可以尽可能地完成更多的交易（多次买卖一支股票）。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

链接：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii

**解题思路**

因为交易次数不受限，如果可以把所有的上坡全部收集到，一定是利益最大化的。

```python
class Solution(object):
    def maxProfit(self, prices):
        profit = 0        
        for i in range(len(prices)-1):
            if prices[i+1] > prices[i]:
                profit += (prices[i+1] - prices[i])
        return profit
```