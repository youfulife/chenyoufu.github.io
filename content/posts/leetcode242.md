---
title: "242 有效的字母异位词"
date: 2021-06-19T23:49:28+08:00
draft: false
tags: ["算法", "字符串"]
categories: ["技术"]
---

**题目**
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

示例 1:
```
输入: s = "anagram", t = "nagaram"
输出: true
```

链接：https://leetcode-cn.com/problems/valid-anagram

**解题思路**

**哈希**

```python
class Solution(object):
    def isAnagram(self, s, t):
        if len(s) != len(t):
            return False
        d1 = {}
        for x in s:
            d1[x] = d1.get(x, 0) + 1
        for x in t:
            if x not in d1 or d1[x] == 0:
                return False
            d1[x] -= 1
        return True
```

**排序**

```python
class Solution(object):
    def isAnagram(self, s, t):
        if len(s) != len(t):
            return False
        s = "".join(sorted(s)) # python字符串没有直接的sort方法
        t = "".join(sorted(t))
        for i in range(len(s)):
            if s[i] != t[i]:
                return False
        return True
```