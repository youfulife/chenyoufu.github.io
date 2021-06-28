---
title: "14 最长公共前缀"
date: 2021-06-26T22:31:40+08:00
draft: false
tags: ["算法", "字符串", "分治"]
categories: ["技术"]
---
**题目**

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

示例 1：
```
输入：strs = ["flower","flow","flight"]
输出："fl"
```
示例 2：
```
输入：strs = ["dog","racecar","car"]
输出：""
解释：输入不存在公共前缀。
```

链接：https://leetcode-cn.com/problems/longest-common-prefix

**解题思路**

**暴力穷举 O(N*N)**

```python
class Solution(object):
    def longestCommonPrefix(self, strs):
        ret = ""
        i = 0
        while True:
            cur = ""
            for s in strs:
                if i == len(s):
                    return ret
                if cur == "":
                    cur = s[i]
                elif cur != s[i]:
                    return ret
            i += 1
            ret += cur
        return ret
```

**找到最短的**

```python
 def longestCommonPrefix(self, strs):
        if not strs:
            return ""
        shortest = min(strs,key=len)
        for i, ch in enumerate(shortest):
            for other in strs:
                if other[i] != ch:
                    return shortest[:i]
        return shortest
```

**字典序排序，比较最小和最大即可**

```python
class Solution(object):
    def longestCommonPrefix(self, strs):
        s1 = min(strs)
        s2 = max(strs)

        l = min(len(s1), len(s2))
        i = 0
        while i < l:
            if s1[i] != s2[i]:
                break
            i += 1
        return s1[:i]
```

**分治**
```python
class Solution(object):
    def longestCommonPrefix(self, strs):
        # 分治
        left = 0
        right = len(strs)-1
        return self.longestCommonPrefixTwo(strs, left, right)

    def longestCommonPrefixTwo(self, strs, left, right):
        if left == right:
            return strs[left]
        
        mid = left +(right-left)/2
        s = self.longestCommonPrefixTwo(strs, left, mid)
        t = self.longestCommonPrefixTwo(strs, mid+1, right)

        l = min(len(s), len(t))
        i = 0
        while i < l:
            if s[i] != t[i]:
                break
            i += 1
        return s[:i]
```