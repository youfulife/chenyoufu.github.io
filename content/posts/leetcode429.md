---
title: "429 N 叉树的层序遍历"
date: 2021-08-10T22:11:29+08:00
draft: false
tags: ["算法", "树", "BFS"]
categories: ["技术"]
---

**题目**

给定一个 N 叉树，返回其节点值的层序遍历。（即从左到右，逐层遍历）。

树的序列化输入是用层序遍历，每组子节点都由 null 值分隔（参见示例）。

**解题思路**

BFS 套路模版

```python
class Solution(object):
    def levelOrder(self, root):
        if not root:
            return []
        ans = []
        q = deque([root])
        step = 0
        while q:
            level = []
            step += 1
            qsize = len(q)
            for _ in range(qsize):
                n = q.popleft()
                level.append(n.val)
                for c in n.children:
                    q.append(c)
            ans.append(level)
        return ans
```