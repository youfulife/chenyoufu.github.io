---
title: "13 罗马数字转整数"
date: 2021-07-07T22:27:41+08:00
draft: false
tags: ["算法", "字符串", "再刷"]
categories: ["技术"]
---

**解题思路**

```python
class Solution(object):
    def romanToInt(self, s):
        ans = 0
        d = {
            'I':             1,
            'V':             5,
            'X':             10,
            'L':             50,
            'C':             100,
            'D':             500,
            'M':             1000,
        }
        for i in range(len(s)):
            cur = d[s[i]]
            if i < len(s)-1 and d[s[i]]< d[s[i+1]]:
                cur *= -1
            
            ans += cur
        return ans
```