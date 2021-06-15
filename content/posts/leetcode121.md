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