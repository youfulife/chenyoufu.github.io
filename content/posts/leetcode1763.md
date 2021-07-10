---
title: "1763 最长的美好子字符串"
date: 2021-07-10T19:59:05+08:00
draft: false
tags: ["算法", "字符串", "递归", "再刷"]
categories: ["技术"]
---
**题目**
当一个字符串 s 包含的每一种字母的大写和小写形式 同时 出现在 s 中，就称这个字符串 s 是 美好 字符串。比方说，"abABB" 是美好字符串，因为 'A' 和 'a' 同时出现了，且 'B' 和 'b' 也同时出现了。然而，"abA" 不是美好字符串因为 'b' 出现了，而 'B' 没有出现。

给你一个字符串 s ，请你返回 s 最长的 美好子字符串 。如果有多个答案，请你返回 最早 出现的一个。如果不存在美好子字符串，请你返回一个空字符串。

示例 1：
```
输入：s = "YazaAay"
输出："aAa"
解释："aAa" 是一个美好字符串，因为这个子串中仅含一种字母，其小写形式 'a' 和大写形式 'A' 也同时出现了。
"aAa" 是最长的美好子字符串。
```

链接：https://leetcode-cn.com/problems/longest-nice-substring

**解题思路**

**暴力穷举**

```python
class Solution(object):
    def longestNiceSubstring(self, s):
        ans = 0
        maxsub = ""
        for i in range(len(s)):
            for j in range(i+1, len(s)):
                sub = s[i:j+1]
                if self.isNice(sub):
                    maxsub = max(maxsub, sub, key=len)
        return maxsub

    def isNice(self, s):
        if len(s) < 2:
            return False
        for c in s:
            if c.swapcase() not in s:
                return False
        return True
```

**分治法**

类似于二叉树,递归终止条件：
1. 如果s的长度小于2那么肯定不可能是美好字符串。
2. 这个字符串就是美好字符串

递归：
* 如果出现了某个字母的大写没有小写，或者出现了小写没有大写，那么所有包含这个字符串的子串都不是美好字符串。也就是说美好字符串只能出现在这个字符串的左侧或者右侧。
* 将左子串和右子串分别递归，按照长度取返回的最大值。注意先传左边的这样遇到相同长度也总是返回先出现的。


```python
class Solution(object):
    def longestNiceSubstring(self, s):
        if len(s) < 2:
            return ""
        for i in range(len(s)):
            c = s[i]
            if (c.islower() and c.upper() not in s) or (c.isupper() and c.lower() not in s):
                left = self.longestNiceSubstring(s[:i])
                right = self.longestNiceSubstring(s[i+1:])
                if len(left) >= len(right):
                    return left
                else:
                    return right
        return s
```

简化下代码

```python
class Solution(object):
    def longestNiceSubstring(self, s):
        if len(s) < 2:
            return ""
        for i in range(len(s)):
            c = s[i]
            if c.swapcase() not in s:
                left = self.longestNiceSubstring(s[:i])
                right = self.longestNiceSubstring(s[i+1:])
                return max(left, right, key=len)
        return s
```