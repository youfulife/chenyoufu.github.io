---
title: "344 反转字符串"
date: 2021-06-19T22:38:00+08:00
draft: false
tags: ["算法", "字符串", "双指针", "反转"]
categories: ["技术"]
---
**题目**

写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。


示例 1：
```
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

链接：https://leetcode-cn.com/problems/reverse-string

**解题思路**

**双指针，头尾交换**

```python
class Solution(object):
    def reverseString(self, s):
        left = 0
        right = len(s) - 1
        while left < right:
            s[left], s[right] = s[right], s[left]
            left+=1
            right-=1
        return s
```