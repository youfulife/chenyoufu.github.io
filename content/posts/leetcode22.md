---
title: "22 括号生成"
date: 2021-05-16T22:23:46+08:00
draft: false
tags: ["算法", "回溯"]
categories: ["技术"]
---

数字 n 代表生成括号的对数，请你设计一个函数，用于能够生成所有可能的并且 有效的 括号组合。

**示例 1：**

```
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```
**示例 2：**

```
输入：n = 1
输出：["()"]
```

链接：https://leetcode-cn.com/problems/generate-parentheses

**解题思路**

满足条件：

```python
class Solution(object):
    def generateParenthesis(self, n):
        ans = []
        def helper(left, right, s):
            if left == n and right == n:
                ans.append(s)
                return
            
            if left < n:
                helper(left+1, right, s + '(')
            if right < left:
                helper(left, right+1, s + ')')
        helper(0, 0, "")
        return ans
```
