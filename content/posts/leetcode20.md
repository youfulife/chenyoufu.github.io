---
title: "20 有效的括号"
date: 2021-04-03T22:03:38+08:00
draft: false
tags: ["算法", "字符串"]
categories: ["技术"]
---
**题目**
给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。

**示例 1：**
```
输入：s = "()"
输出：true
```
**示例 2：**
```
输入：s = "()[]{}"
输出：true
```

链接：https://leetcode-cn.com/problems/valid-parentheses

**解题思路**

基于栈来进行匹配。

提交了4次才通过，每次都有没有考虑到的地方。

解决边界问题：
* 栈 stack 为空： 此时 stack.pop() 操作会报错
* 字符串 s 以左括号结尾： 此情况下可以正常遍历完整个 s，但 stack 中遗留未出栈的左括号

```python
class Solution(object):
    def isValid(self, s):
        if len(s) <= 1:
            return False

        stack = []
        for x in s:
            if x in ['(', '[', '{']:
                stack.append(x)
            else:
                if len(stack) == 0: # 这个地方第三次提交没检查
                    return False
                c = stack.pop()
                if x == ')':
                    if c != '(':
                        return False
                elif x == ']':
                    if c != '[':
                        return False
                else:
                    if c != '{': # 第一次提交 写成了 '}'
                        return False
        if len(stack) > 0: # 第二次提交没检查
            return False
        return True
```

简化下代码

```python

class Solution(object):
    def isValid(self, s):
        if len(s) % 2 != 0: # 优化1
            return False

        d = { # 优化2
            '(': ')',
            '[': ']',
            '{': '}'
        }

        stack = []
        for x in s:
            if x in d:
                stack.append(x)
            else:
                if len(stack) == 0 or x != d[stack.pop()]:
                    return False
        return len(stack) == 0
```