---
title: "438 找到字符串中所有字母异位词"
date: 2021-07-21T22:51:09+08:00
draft: false
tags: ["算法", "滑动窗口"]
categories: ["技术"]
---

**题目**

给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

异位词 指字母相同，但排列不同的字符串。

示例 1:
```
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
```

链接：https://leetcode-cn.com/problems/find-all-anagrams-in-a-string

**解题思路**

```python
class Solution(object):
    def findAnagrams(self, s, p):
        need = {}
        for x in p:
            need[x] = need.get(x, 0) + 1
        
        window = {}
        left, right = 0, 0
        valid = 0
        ans = []
        while right < len(s):
            c = s[right]
            right += 1
            window[c] = window.get(c, 0) + 1

            if c not in need:
                left = right
                valid = 0
                window = {}
                continue

            if window[c] == need[c]:
                valid += 1

            if right - left < len(p):
                continue
            
            while valid == len(need):
                if right-left == len(p):
                    ans.append(left)
            
                d = s[left]
                left += 1
                if window[d] == need[d]:
                    valid -= 1
                window[d] -= 1
        return ans
```

优化流程, 转化为标准滑动窗口

```python
class Solution(object):
    def findAnagrams(self, s, p):
        need = {}
        for x in p:
            need[x] = need.get(x, 0) + 1
        
        window = {}
        left, right = 0, 0
        ans = []
        while right < len(s):
            c = s[right]
            right += 1
            window[c] = window.get(c, 0) + 1

            while window[c] > need.get(c, 0):
                d = s[left]
                left += 1
                window[d] -= 1
                
            if right-left == len(p):
                ans.append(left)
            
        return ans
```