---
title: "1202 交换字符串中的元素"
date: 2021-08-07T18:09:25+08:00
draft: false
tags: ["算法", "并查集"]
categories: ["技术"]
---

**题目**

你一个字符串 s，以及该字符串中的一些「索引对」数组 pairs，其中 pairs[i] = [a, b] 表示字符串中的两个索引（编号从 0 开始）。

你可以 任意多次交换 在 pairs 中任意一对索引处的字符。

返回在经过若干次交换后，s 可以变成的按字典序最小的字符串。


示例 1:
```
输入：s = "dcab", pairs = [[0,3],[1,2]]
输出："bacd"
解释： 
交换 s[0] 和 s[3], s = "bcad"
交换 s[1] 和 s[2], s = "bacd"
```

链接：https://leetcode-cn.com/problems/smallest-string-with-swaps

**解题思路**

并查集

```python
class Solution(object):
    class UFS(object):
        def __init__(self, n, s):
            self.count = n
            self.p = [i for i in range(n)]
        def find(self, x):
            if x != self.p[x]:
                self.p[x] = self.find(self.p[x])
            return self.p[x]
        def union(self, x, y):
            x_root = self.find(x)
            y_root = self.find(y)
            if x_root == y_root:
                return
            self.p[x_root] = y_root
            return
        def isConnected(self, x, y):
            return self.find(x) == self.find(y)

    def smallestStringWithSwaps(self, s, pairs):
        n = len(s)
        ufs = Solution.UFS(n, s)
        for pair in pairs:
            ufs.union(pair[0], pair[1])
        # 扎堆分类（通过相同的父节点，将同一个集合的分在一起）
        m = {}
        for i in range(n):
            root = ufs.find(i)
            m[root] = m.get(root, [])
            m[root].append(s[i])
        # 排序
        for k in m:
            m[k] = sorted(m[k], reverse=True)

        ans = []
        for i in range(n):
            root = ufs.find(i)
            minc = m[root][-1]
            ans.append(minc)
            m[root].pop()

        return ''.join(ans)
```