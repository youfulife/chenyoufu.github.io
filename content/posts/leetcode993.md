---
title: "993 二叉树的堂兄弟节点"
date: 2021-08-10T22:30:30+08:00
draft: false
tags: ["算法", "树", "BFS"]
categories: ["技术"]
---

**题目**

在二叉树中，根节点位于深度 0 处，每个深度为 k 的节点的子节点位于深度 k+1 处。

如果二叉树的两个节点深度相同，但 父节点不同 ，则它们是一对堂兄弟节点。

我们给出了具有唯一值的二叉树的根节点 root ，以及树中两个不同节点的值 x 和 y 。

只有与值 x 和 y 对应的节点是堂兄弟节点时，才返回 true 。否则，返回 false。

链接：https://leetcode-cn.com/problems/cousins-in-binary-tree

**解题思路**

BFS, 每一层判断是否满足条件

```python
class Solution(object):
    def isCousins(self, root, x, y):
        q = deque([(root, None)])
        found = 0
        xp = None
        yp = None
        while q:
            level = []
            for _ in range(len(q)):
                n, p = q.popleft()
                level.append((n.val, p))
                if n.val == x:
                    found += 1
                    xp = p
                if n.val == y:
                    found += 1
                    yp = p
                if n.left:
                    q.append((n.left, n))
                if n.right:
                    q.append((n.right, n))
            if found == 2 and xp != yp:
                return True
            elif found == 1:
                return False
        return False
```