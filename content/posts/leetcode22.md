---
title: "22 括号生成"
date: 2021-05-16T22:23:46+08:00
draft: false
tags: ["算法", "回溯", "DFS", "BFS", "排列组合", "动态规划", "五星"]
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

**深度优先**

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

**国际站讨论区最高赞答案**

```java
public List<String> generateParenthesis(int n) {
        List<String> list = new ArrayList<String>();
        backtrack(list, "", 0, 0, n);
        return list;
    }
    
    public void backtrack(List<String> list, String str, int open, int close, int max){
        
        // 这个判断更巧妙
        if(str.length() == max*2){
            list.add(str);
            return;
        }
        
        if(open < max)
            backtrack(list, str+"(", open+1, close, max);
        if(close < open)
            backtrack(list, str+")", open, close+1, max);
    }
```

**广度优先**

```python
class Solution(object):
    def generateParenthesis(self, n):
        ans = []

        q = [('', 0, 0)]
        for s, left, right in q:
            if left == n and right == n:
                ans.append(s)
            if left < n:
                q.append((s + '(', left + 1, right))
            if right < left:
                q.append((s + ')', left, right + 1))

        return ans
```

**动态规划**