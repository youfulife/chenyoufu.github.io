---
title: "剑指 Offer 50 第一个只出现一次的字符"
date: 2021-05-29T10:49:30+08:00
draft: false
tags: ["算法", "第一个"]
categories: ["技术"]
---

**题目**
在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

**示例:**

```
s = "abaccdeff"
返回 "b"

s = "" 
返回 " "
```
链接：https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof

**解题思路**

先把字符串中每个字符出现的次数放到一个dict中，再遍历字符串，判断次数是1的直接返回。

```python
class Solution():
    def firstUniqChar(self, s):
        d = {}
        for c in s:
            d[c] = d.get(c, 0) + 1
        
        for c in s:
            if d[c] == 1:
                return c
        
        return " "
```