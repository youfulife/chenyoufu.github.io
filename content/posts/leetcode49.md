---
title: "49 字母异位词分组"
date: 2021-04-10T21:17:14+08:00
draft: false
tags: ["算法", "哈希表"]
categories: ["技术"]
---

给定一个字符串数组，将字母异位词组合在一起。字母异位词指字母相同，但排列不同的字符串。

**示例:**

```
输入: ["eat", "tea", "tan", "ate", "nat", "bat"]
输出:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

**说明：**

所有输入均为小写字母。
不考虑答案输出的顺序。

**链接：** https://leetcode-cn.com/problems/group-anagrams

**解答思路**

**1. 排序 + 哈希表**

V1 自己第一遍写的版本

```python
class Solution(object):
    def groupAnagrams(self, strs):
        d = {}
        for s in strs:
            k = ''.join(sorted(s))
            d[k] = d.get(k, [])
            d[k].append(s)
        return d.values()
```

V2 看国际站评论后改进后的版本

```python
class Solution(object):
    def groupAnagrams(self, strs):
        d = {}
        for s in strs:
            k = tuple(sorted(s))
            d[k] = d.get(k, []) + [s]
        return d.values()
```

V3 defaultdict

```python
class Solution(object):
    def groupAnagrams(self, strs):
        ans = collections.defaultdict(list)
        for s in strs:
            ans[tuple(sorted(s))].append(s)
        return ans.values()
```

**2. 计数 + 哈希表**

```python
class Solution(object):
    def groupAnagrams(self, strs):
        d = {}
        for s in strs:
            c = [0] * 26
            for b in s:
                c[ord(b) - ord('a')] += 1
            d[tuple(c)] = d.get(tuple(c), []) + [s]
        return d.values()
```