---
title: "76 最小覆盖子串"
date: 2021-07-20T21:57:55+08:00
draft: false
tags: ["算法", "字符串", "滑动窗口"]
categories: ["技术"]
---

**题目**

给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。

示例 1：
```
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
```
示例 2：
```
输入：s = "a", t = "a"
输出："a"
```

注意：

* 对于 t 中重复字符，我们寻找的子字符串中该字符数量必须不少于 t 中该字符数量。
* 如果 s 中存在这样的子串，我们保证它是唯一的答案。


链接：https://leetcode-cn.com/problems/minimum-window-substring

**解题思路**

滑动窗口标准模版

```python
class Solution(object):
    def minWindow(self, s, t):
        valid = {}
        for x in t:
            valid[x] = valid.get(x, 0) + 1
        left = 0
        right = 0
        window = {}
        totalValid = 0
        start = 0
        end = 0
        minL = len(s)
        while right < len(s):
            c = s[right]
            window[c] = window.get(c, 0) + 1
            right += 1

            if c in valid and window[c] == valid[c]:
                totalValid += 1
            
            while totalValid == len(valid):
                if right - left <= minL:
                    start = left
                    end = right
                    minL = right - left

                d = s[left]
                window[d] -= 1
                if d in valid and window[d] < valid[d]:
                    totalValid -= 1
                left += 1
            
        return s[start: end]
```

优化下代码结构，更加清晰

```python
class Solution(object):
    def minWindow(self, s, t):
        need, window = {}, {}
        for x in t:
            need[x] = need.get(x, 0) + 1
        left, right = 0, 0
        valid = 0
        start, end = 0, 0
        minL = len(s) + 1
        while right < len(s):
            c = s[right]
            right += 1
            if c in need:
                window[c] = window.get(c, 0) + 1
                if window[c] == need[c]:
                    valid += 1
            
            while valid == len(need):
                if right - left < minL:
                    start = left
                    end = right
                    minL = right - left
                
                d = s[left]
                left += 1
                if d in need:
                    if window[d] == need[d]:
                        valid -= 1
                    window[d] -= 1
            
        return s[start: end]
```

