---
title: "680 验证回文字符串 Ⅱ"
date: 2021-06-20T12:17:45+08:00
draft: false
tags: ["算法", "字符串", "回文", "贪心", "双指针"]
categories: ["技术"]
---
**题目**

给定一个非空字符串 s，最多删除一个字符。判断是否能成为回文字符串。

示例 1:
```
输入: "aba"
输出: True
```
示例 2:
```
输入: "abca"
输出: True
```
解释: 你可以删除c字符。

注意:

字符串只包含从 a-z 的小写字母。字符串的最大长度是50000。

链接：https://leetcode-cn.com/problems/valid-palindrome-ii

**解题思路**

**贪心**

如果两个指针指向的字符不同，则两个字符中必须有一个被删除，此时我们就分成两种情况：即删除左指针对应的字符或者删除右指针对应的字符。当这两个子串中至少有一个是回文串时，就说明原始字符串删除一个字符之后就以成为回文串。

```python
class Solution(object):
    def validPalindrome(self, s):
        
        def isPalindrome(left, right):
            while left < right:
                if s[left] != s[right]:
                    return False
                left += 1
                right -= 1
            return True
        
        left = 0
        right = len(s)-1
        while left < right:
            if s[left] != s[right]:
                return isPalindrome(left+1, right) or isPalindrome(left, right-1)
            left += 1
            right -= 1
        return True
```