---
title: "513 找树左下角的值"
date: 2021-04-17T13:46:56+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

**题目**

给定一个二叉树，在树的最后一行找到最左边的值。

**示例 1:**
```
输入:

    2
   / \
  1   3

输出:
1
```

**示例 2:**

```
输入:

        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

输出:
7
```

注意: 您可以假设树（即给定的根节点）不为 NULL。

链接：https://leetcode-cn.com/problems/find-bottom-left-tree-value


**解题思路**

1. 层序遍历，返回最下层的第一个元素的值。

```python
class Solution(object):
    def findBottomLeftValue(self, root):
        q = deque([root])
        levels = []
        while q:
            level = []
            for _ in range(len(q)):
                node = q.popleft()
                level.append(node.val)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            levels.append(level)
        return levels[-1][0]
```

2. 看评论后优化，还是层序遍历，只不过修改成从右往左，这样只要把队列的最后一个元素返回就可以了，非常巧妙。

```python
class Solution(object):
    def findBottomLeftValue(self, root):
        q = deque([root])
        node = root
        while q:
            node = q.popleft()
            if node.right:
                q.append(node.right)
            if node.left:
                q.append(node.left)
        return node.val
```
