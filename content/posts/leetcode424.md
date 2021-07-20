---
title: "424 替换后的最长重复字符"
date: 2021-07-20T22:15:37+08:00
draft: false
tags: ["算法", "字符串", "滑动窗口"]
categories: ["技术"]
---

**题目**

给你一个仅由大写英文字母组成的字符串，你可以将任意位置上的字符替换成另外的字符，总共可最多替换 k 次。在执行上述操作后，找到包含重复字母的最长子串的长度。

注意：字符串长度 和 k 不会超过 104。

示例 1：
```
输入：s = "ABAB", k = 2
输出：4
解释：用两个'A'替换为两个'B',反之亦然。
```

链接：https://leetcode-cn.com/problems/longest-repeating-character-replacement

**解题思路**

* 滑动窗口，枚举字符串中的每一个位置作为右端点，然后找到其最远的左端点的位置，满足该区间内除了出现次数最多的那一类字符之外，剩余的字符（即非最长重复字符）数量不超过 k 个。


```python
class Solution(object):
    def characterReplacement(self, s, k):
        left = 0
        right = 0
        window = {}
        valid = 0
        ans = 0
        while right < len(s):
            c = s[right]
            right += 1
            window[c] = window.get(c, 0) + 1

            valid = sum(window.values()) - max(window.values())
    
            while valid > k:
                d = s[left]
                left += 1
                window[d] -= 1
                valid = sum(window.values()) - max(window.values())
            ans = max(ans, sum(window.values()))
        return ans
```

优化代码，提高下性能

```python
class Solution(object):
    def characterReplacement(self, s, k):
        left = 0
        right = 0
        ans = 0
        window = {}
        maxV = 0
        while right < len(s):
            c = s[right]
            right += 1
            window[c] = window.get(c, 0) + 1

            maxV = max(window.values())
    
            if right - left > maxV + k:
                d = s[left]
                left += 1
                window[d] -= 1
            ans = max(ans, right - left)
        return ans
```