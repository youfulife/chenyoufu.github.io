---
title: "1208 尽可能使字符串相等"
date: 2021-07-23T22:20:45+08:00
draft: false
tags: ["算法", "滑动窗口"]
categories: ["技术"]
---

**题目**
给你两个长度相同的字符串，s 和 t。

将 s 中的第 i 个字符变到 t 中的第 i 个字符需要 |s[i] - t[i]| 的开销（开销可能为 0），也就是两个字符的 ASCII 码值的差的绝对值。

用于变更字符串的最大预算是 maxCost。在转化字符串时，总开销应当小于等于该预算，这也意味着字符串的转化可能是不完全的。

如果你可以将 s 的子字符串转化为它在 t 中对应的子字符串，则返回可以转化的最大长度。

如果 s 中没有子字符串可以转化成 t 中对应的子字符串，则返回 0。

示例 1：
```
输入：s = "abcd", t = "bcdf", maxCost = 3
输出：3
解释：s 中的 "abc" 可以变为 "bcd"。开销为 3，所以最大长度为 3。
```
链接：https://leetcode-cn.com/problems/get-equal-substrings-within-budget

**解题思路**

标准滑动窗口模版题

```python
class Solution(object):
    def equalSubstring(self, s, t, maxCost):
        left = 0
        right = 0
        cost = 0
        ans = 0
        while right < len(s):
            c, d = s[right], t[right]
            right += 1
            cost += abs(ord(c)-ord(d))

            while cost > maxCost:
                c, d = s[left], t[left]
                left += 1
                cost -= abs(ord(c)-ord(d))
            
            ans = max(ans, right-left)
        return ans
```