---
title: "1456 定长子串中元音的最大数目"
date: 2021-07-16T23:33:57+08:00
draft: false
tags: ["算法", "字符串", "滑动窗口"]
categories: ["技术"]
---
**题目**

给你字符串 s 和整数 k 。

请返回字符串 s 中长度为 k 的单个子字符串中可能包含的最大元音字母数。

英文中的 元音字母 为（a, e, i, o, u）。

示例 1：
```
输入：s = "abciiidef", k = 3
输出：3
解释：子字符串 "iii" 包含 3 个元音字母。
```

链接：https://leetcode-cn.com/problems/maximum-number-of-vowels-in-a-substring-of-given-length

**解题思路**

**标准的定长滑动窗口套路**

```python
class Solution(object):
    def maxVowels(self, s, k):
        target = {
            "a": 1,
            "e": 1,
            "i": 1,
            "o": 1,
            "u": 1,
        }
        valid = 0
        for i in range(k):
            if s[i] in target:
                valid += 1
        ans = valid
        for i in range(k, len(s)):
            if s[i] in target:
                valid += 1
            if s[i-k] in target:
                valid -= 1
            ans = max(ans, valid)
        return ans
```