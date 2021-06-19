---
title: "387 字符串中的第一个唯一字符"
date: 2021-06-19T23:30:06+08:00
draft: false
tags: ["算法", "字符串"]
categories: ["技术"]
---

**题目**

给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

示例：
```
s = "leetcode"
返回 0

s = "loveleetcode"
返回 2
```

提示：你可以假定该字符串只包含小写字母。

**解题思路**

* hash表记录出现的次数，循环两遍

```python
class Solution(object):
    def firstUniqChar(self, s):
        d = {}
        for x in s:
            d[x] = d.get(x, 0) + 1
        for i, x in enumerate(s):
            if d[x] == 1:
                return i
        return -1 
```

**队列**

我们也可以借助队列找到第一个不重复的字符。队列具有「先进先出」的性质，因此很适合用来找出第一个满足某个条件的元素。

```python
class Solution(object):
    def firstUniqChar(self, s):
        d = {}
        q = deque()
        for i, x in enumerate(s):
            if x in d:
                d[x] = -1
                while q and d[q[0][0]] == -1:
                    q.popleft()
            else:
                d[x] = 1
                q.append((x, i))
        return q[0][1] if q else -1
```