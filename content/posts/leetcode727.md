---
title: "727 最小窗口子序列"
date: 2021-07-24T10:28:55+08:00
draft: false
tags: ["算法", "滑动窗口", "五星"]
categories: ["技术"]
---

**题目**

给定字符串 S and T，找出 S 中最短的（连续）子串 W ，使得 T 是 W 的 子序列 。

如果 S 中没有窗口可以包含 T 中的所有字符，返回空字符串 ""。如果有不止一个最短长度的窗口，返回开始位置最靠左的那个。

示例 1：
```
输入：
S = "abcdebdde", T = "bde"
输出："bcde"
解释：
"bcde" 是答案，因为它在相同长度的字符串 "bdde" 出现之前。
"deb" 不是一个更短的答案，因为在窗口中必须按顺序出现 T 中的元素。
```

链接：https://leetcode-cn.com/problems/minimum-window-subsequence

**解题思路**

* 这个题和 76 最小覆盖子串 类似，不过最小覆盖子串不要求顺序，这个题目要求顺序，感觉更加难一些，自己想没有想出来。
* 看题解是先匹配到最长的右边界，然后逆向找滑动窗口的左边界，保证长度最小。

```python

class Solution(object):
    def minWindow(self, s, t):
        # j是t的索引
        j = 0
        # 窗口边界
        left, right = 0, 0
        # 返回结果
        start, minL = 0, len(s) + 1
        while right < len(s):
            if s[right] == t[j]:
                j += 1
            right += 1
            # 满足条件, 逆向寻找左边界
            if j == len(t):
                left = right - 1
                j -= 1
                while j >= 0:
                    if s[left] == t[j]:
                        j -= 1
                    left -= 1
                # 维持左闭右开
                left += 1
                if right - left < minL:
                    start = left
                    minL = right - left
                # 下一个可能的位置，感觉这里还可以有些优化
                right = left + 1
                
        return s[start: start+minL] if minL < len(s) + 1 else ""
```