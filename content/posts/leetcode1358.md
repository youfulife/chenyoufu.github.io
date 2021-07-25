---
title: "1358 包含所有三种字符的子字符串数目"
date: 2021-07-25T17:33:00+08:00
draft: false
tags: ["算法", "滑动窗口", "五星", "前缀和"]
categories: ["技术"]
---

**题目**

给你一个字符串 s ，它只包含三种字符 a, b 和 c 。

请你返回 a，b 和 c 都 至少 出现过一次的子字符串数目。

示例 1：
```
输入：s = "abcabc"
输出：10
解释：包含 a，b 和 c 各至少一次的子字符串为 "abc", "abca", "abcab", "abcabc", "bca", "bcab", "bcabc", "cab", "cabc" 和 "abc" (相同字符串算多次)。
```

链接：https://leetcode-cn.com/problems/number-of-substrings-containing-all-three-characters

**解题思路**

* 滑动窗口+前缀和
* exactly(3) = atMost(3) - atMost(2)

```python
class Solution(object):
    def numberOfSubstrings(self, s):
        def atMost(s, k):
            left, right = 0, 0
            window = {}
            valid = 0
            ans = 0

            while right < len(s):
                c= s[right]
                right += 1
                window[c] = window.get(c, 0) + 1
                if window[c] == 1:
                    valid += 1
                while valid > k:
                    d = s[left]
                    left += 1
                    if window[d] == 1:
                        valid -= 1
                    window[d] -= 1
                ans += right - left
            return ans
        return atMost(s, 3) - atMost(s, 2)
```

**滑动窗口**

```python
class Solution(object):
    def numberOfSubstrings(self, s):
        left, right = 0, 0
        window = {}
        valid = 0
        ans = 0
        n = len(s)
        k = 3

        while right < n:
            c= s[right]
            right += 1
            window[c] = window.get(c, 0) + 1
            if window[c] == 1:
                valid += 1
            while valid == k:
                # 以left为边界，以left开头的满足条件的所有子序列
                ans += n-right + 1
                d = s[left]
                left += 1
                if window[d] == 1:
                    valid -= 1
                window[d] -= 1
            
        return ans
```