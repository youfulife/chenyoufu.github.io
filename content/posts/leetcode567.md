---
title: "567 字符串的排列"
date: 2021-07-22T22:47:24+08:00
draft: false
tags: ["算法", "滑动窗口"]
categories: ["技术"]
---

**题目**

给定两个字符串 s1 和 s2，写一个函数来判断 s2 是否包含 s1 的排列。

换句话说，第一个字符串的排列之一是第二个字符串的 子串 。

示例 1：
```
输入: s1 = "ab" s2 = "eidbaooo"
输出: True
解释: s2 包含 s1 的排列之一 ("ba").
```

链接：https://leetcode-cn.com/problems/permutation-in-string

**解题思路**

```python
class Solution(object):
    def checkInclusion(self, s1, s2):
        need = {}
        for x in s1:
            need[x] = need.get(x, 0) + 1

        left, right = 0, 0
        window = {}
        valid = 0
        while right < len(s2):
            c = s2[right]
            right += 1
            window[c] = window.get(c, 0) + 1
            if c in need and window[c] == need[c]:
                valid += 1

            while right - left > len(s1):
                d = s2[left]
                left += 1
                if d in need and window[d] == need[d]:
                    valid -= 1
                window[d] -= 1
                if window[d] == 0:
                    del window[d]

            if valid == len(need) and right-left == len(s1):
                return True
        return False
```

* 跟 438 找到字符串中所有字母异位词 https://youfu.life/posts/leetcode438/ 其实是同一个。
* 只要保证窗口内都是满足条件的字符就可以。

```python
class Solution(object):
    def checkInclusion(self, s1, s2):
        need = {}
        for x in s1:
            need[x] = need.get(x, 0) + 1
        left, right = 0, 0
        window = {}
        while right < len(s2):
            c = s2[right]
            right += 1
            window[c] = window.get(c, 0) + 1

            while window[c] > need.get(c, 0):
                d = s2[left]
                left += 1
                window[d] -= 1

            if right-left == len(s1):
                return True
        return False
```