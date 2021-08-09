---
title: "103 二叉树的锯齿形层序遍历"
date: 2021-08-09T23:26:32+08:00
draft: false
tags: ["算法", "树", "BFS", "五星"]
categories: ["技术"]
---

**题目**

给定一个二叉树，返回其节点值的锯齿形层序遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

例如：
```
给定二叉树 [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回锯齿形层序遍历如下：

[
  [3],
  [20,9],
  [15,7]
]
```

链接：https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal


**解题思路**

BFS + 倒序

```python
class Solution(object):
    def zigzagLevelOrder(self, root):
        if not root:
            return []
        
        ans = []
        levelIndex = 0
        q = deque([root])
        while q:
            level = []
            for _ in range(len(q)):
                node = q.popleft()
                level.append(node.val)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            # 根据奇偶进行倒序输出
            if levelIndex % 2 == 0:
                ans.append(level)
            else:
                ans.append(level[::-1])
            levelIndex += 1
        return ans               
```