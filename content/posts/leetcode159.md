---
title: "159 至多包含两个不同字符的最长子串"
date: 2021-07-10T21:54:06+08:00
draft: false
tags: ["算法", "字符串", "滑动窗口"]
categories: ["技术"]
---

**题目**

给定一个字符串 s ，找出 至多 包含两个不同字符的最长子串 t ，并返回该子串的长度。

示例 1:
```
输入: "eceba"
输出: 3
解释: t 是 "ece"，长度为3。
```

链接：https://leetcode-cn.com/problems/longest-substring-with-at-most-two-distinct-characters

**解题思路**

**滑动窗口**

```python
class Solution(object):
    def lengthOfLongestSubstringTwoDistinct(self, s):
        # 窗口是开区间[left, right)
        left = 0
        right = 0
        window = {}
        ans = 0
        while right < len(s):
            c = s[right]
            window[c] = window.get(c, 0) + 1
            right += 1
            while len(window) > 2:
                x = s[left]
                window[x] -= 1
                if window[x] == 0:
                    del window[x]
                left += 1
            ans = max(ans, right-left)
        return ans
```

解法二，额外加一个变量记录满足条件的个数

```python
class Solution:
    def lengthOfLongestSubstringTwoDistinct(self, s: str) -> int:
        n = len(s)
        chr_cnt = defaultdict(int)
        kind = 0
        ######## 滑动窗口
        L = 0
        res = 0
        # 窗口是闭区间[left, right]
        for R in range(n):
            #### (1)进R
            if chr_cnt[s[R]] == 0:
                kind += 1
            chr_cnt[s[R]] += 1
            #### (2)弹L
            while kind > 2:
                chr_cnt[s[L]] -= 1
                if chr_cnt[s[L]] == 0:
                    kind -= 1
                L += 1
            #### (3)更新res, 针对不同的R这里决定是否要+1
            res = max(res, R - L + 1)
        
        return res
```