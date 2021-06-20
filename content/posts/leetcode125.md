---
title: "125 验证回文串"
date: 2021-06-20T11:58:11+08:00
draft: false
tags: ["算法", "字符串", "回文"]
categories: ["技术"]
---
**题目**
给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

说明：本题中，我们将空字符串定义为有效的回文串。

示例 1:
```
输入: "A man, a plan, a canal: Panama"
输出: true
```

链接：https://leetcode-cn.com/problems/valid-palindrome

**解题思路**

* 双指针对比头尾字符

```python
class Solution(object):
    def isPalindrome(self, s):
        left = 0
        right = len(s) - 1
        while left < right:
            if not s[left].isalnum():
                left += 1
                continue
            if not s[right].isalnum():
                right -= 1
                continue
            if s[left].lower() != s[right].lower():
                return False
            left += 1
            right -= 1
        return True
```

* 去掉其他字符，反转字符串，判断反转后的字符串是否相同

```python
class Solution(object):
    def isPalindrome(self, s):
        y = ""
        for x in s:
            if x.isalnum():
                y += x.lower()
        return y == y[::-1]
```
