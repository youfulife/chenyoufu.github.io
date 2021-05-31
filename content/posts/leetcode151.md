---
title: "151 翻转字符串里的单词"
date: 2021-05-31T13:22:45+08:00
draft: false
tags: ["算法", "字符串"]
categories: ["技术"]
---

**题目**

给你一个字符串 s ，逐个翻转字符串中的所有 单词 。

单词 是由非空格字符组成的字符串。s 中使用至少一个空格将字符串中的 单词 分隔开。

请你返回一个翻转 s 中单词顺序并用单个空格相连的字符串。

说明：

输入字符串 s 可以在前面、后面或者单词间包含多余的空格。
翻转后单词间应当仅用一个空格分隔。
翻转后的字符串中不应包含额外的空格。
 

示例 1：
```
输入：s = "the sky is blue"
输出："blue is sky the"
```

**进阶：**

请尝试使用 O(1) 额外空间复杂度的原地解法。

链接：https://leetcode-cn.com/problems/reverse-words-in-a-string

**解题思路**

* 基于空格分割字符串
* 列表反转
* 基于空格join

```python
class Solution(object):
    def reverseWords(self, s):
        words = s.split() # 这里不要用s.split(' '), 这样多个空格会分割出空字符串到列表, 还要再过滤掉.
        return ' '.join(words[::-1])
```



