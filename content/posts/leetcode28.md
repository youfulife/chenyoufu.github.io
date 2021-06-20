---
title: "28 实现 strStr()"
date: 2021-06-20T14:12:07+08:00
draft: false
tags: ["算法", "字符串"]
categories: ["技术"]
---

**题目**

实现 strStr() 函数。

给你两个字符串 haystack 和 needle ，请你在 haystack 字符串中找出 needle 字符串出现的第一个位置（下标从 0 开始）。如果不存在，则返回  -1 。

链接：https://leetcode-cn.com/problems/implement-strstr

**解题思路**

**暴力匹配**

```python
class Solution(object):
    def strStr(self, haystack, needle):
        if len(needle) == 0:
            return 0
        if len(haystack) < len(needle):
            return -1
        
        for i in range(len(haystack) - len(needle) + 1):
            if haystack[i: i + len(needle)] == needle:
                return i
        return -1
```

**KMP 解法**

KMP 算法是一个快速查找匹配串的算法，它的作用其实就是本题问题：如何快速在「原字符串」中找到「匹配字符串」。

上述的朴素解法，不考虑剪枝的话复杂度是 O(m * n) 的，而 KMP 算法的复杂度为 O(m + n)。

KMP 之所以能够在 O(m + n)复杂度内完成查找，是因为其能在「非完全匹配」的过程中提取到有效信息进行复用，以减少「重复匹配」的消耗。

