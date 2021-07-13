---
title: "340 至多包含 K 个不同字符的最长子串"
date: 2021-07-12T22:49:12+08:00
draft: false
tags: ["算法", "字符串", "滑动窗口"]
categories: ["技术"]
---

给定一个字符串 s ，找出 至多 包含 k 个不同字符的最长子串 T。

示例 1:
```
输入: s = "eceba", k = 2
输出: 3
解释: 则 T 为 "ece"，所以长度为 3。
```
示例 2:
```
输入: s = "aa", k = 1
输出: 2
解释: 则 T 为 "aa"，所以长度为 2。
```

链接：https://leetcode-cn.com/problems/longest-substring-with-at-most-k-distinct-characters

**解题思路**

标准的滑动窗口模版题目

```python
class Solution(object):
    def lengthOfLongestSubstringKDistinct(self, s, k):
        left = 0
        right = 0
        window = {}
        ans = 0
        while right < len(s):
            c = s[right]
            window[c] = window.get(c, 0) + 1
            right += 1
            
            while len(window) > k:
                d = s[left]
                window[d] -= 1
                if window[d] == 0:
                    del window[d]
                left += 1

            ans = max(ans, right-left)
        return ans
```