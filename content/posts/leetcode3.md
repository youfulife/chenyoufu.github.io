---
title: "3 无重复字符的最长子串"
date: 2021-07-08T23:43:52+08:00
draft: false
tags: ["算法", "字符串", "滑动窗口"]
categories: ["技术"]
---

**题目**

给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。

示例 1:
```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

链接：https://leetcode-cn.com/problems/longest-substring-without-repeating-characters

**解题思路**

1. 类似最长xxx相关的，第一时间想到动态规划，但是没想出来动态转移方程
2. 感觉可以用滑动窗口

```python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        window = {}
        ans = 0
        left = 0 # 左边界
        right = 0 # 右边界 闭区间 []
        while right < len(s):
            c = s[right]
            window[c] = window.get(c, 0) + 1
            if window[c] > 1:
                ans = max(ans, right-left)
                # 收缩到当前重复字符的下一个字符
                while window[c] > 1:
                    window[s[left]] -= 1
                    left += 1
            right += 1
        ans = max(ans, right - left)
        return ans
```

优化下代码，更加简洁

```python
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        window = {}
        ans = 0
        left = 0 # 左边界
        right = 0 # 右边界 闭区间 []
        while right < len(s):
            c = s[right]
            window[c] = window.get(c, 0) + 1
            right += 1
            while window[c] > 1:
                # 收缩到当前重复字符的下一个字符
                window[s[left]] -= 1
                left += 1
            ans = max(ans, right-left)
        ans = max(ans, right - left)
        return ans
```