---
title: "1100 长度为 K 的无重复字符子串"
date: 2021-07-24T10:02:11+08:00
draft: false
tags: ["算法", "滑动窗口"]
categories: ["技术"]
---

**题目**

给你一个字符串 S，找出所有长度为 K 且不含重复字符的子串，请你返回全部满足要求的子串的 数目。

示例 1：
```
输入：S = "havefunonleetcode", K = 5
输出：6
解释：
这里有 6 个满足题意的子串，分别是：'havef','avefu','vefun','efuno','etcod','tcode'。
```

链接：https://leetcode-cn.com/problems/find-k-length-substrings-with-no-repeated-characters

**解题思路**

滑动窗口标准题目，只是比之前最长的无重复子串的题多了一个固定长度的条件。

```python
class Solution(object):
    def numKLenSubstrNoRepeats(self, s, k):
        ans = 0
        left, right = 0, 0
        window = {}
        while right < len(s):
            c = s[right]
            right += 1
            window[c] = window.get(c, 0) + 1

            while window[c] > 1 or len(window) > k:
                d = s[left]
                left += 1
                window[d] -= 1
                if window[d] == 0:
                    del window[d]

            if len(window) == k:
                ans += 1
        return ans
```

其实在窗口收缩的时候可以不判断窗口的长度，就按照最长无重复子串来，计算结果的时候条件修改下就可以了。

```python
class Solution(object):
    def numKLenSubstrNoRepeats(self, s, k):
        ans = 0
        left, right = 0, 0
        window = {}
        while right < len(s):
            c = s[right]
            right += 1
            window[c] = window.get(c, 0) + 1

            while window[c] > 1:
                d = s[left]
                left += 1
                window[d] -= 1
                if window[d] == 0:
                    del window[d]
            # 这里条件要 >= k
            if len(window) >= k:
                ans += 1
        return ans
```