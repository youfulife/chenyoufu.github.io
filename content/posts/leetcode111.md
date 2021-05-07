---
title: "111 二叉树的最小深度"
date: 2021-05-07T20:13:01+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

说明：叶子节点是指没有子节点的节点。
```
输入：root = [3,9,20,null,null,15,7]
输出：2
示例 2：

输入：root = [2,null,3,null,4,null,5,null,6]
输出：5
```

链接：https://leetcode-cn.com/problems/minimum-depth-of-binary-tree

**解题思路**

这个题目有个点就是左子树不存在的情况下，只计算右子树的深度。

**DFS 递归**

```python
class Solution(object):
    def minDepth(self, root):
        if not root:
            return 0
        
        if not root.left or not root.right:
            return 1 + self.minDepth(root.left) + self.minDepth(root.right)
        
        return 1 + min(self.minDepth(root.left), self.minDepth(root.right))     
```

**BFS 层序遍历**

找到的第一个叶子结点即是最小深度

```python
class Solution(object):
    def minDepth(self, root):
        if not root:
            return 0
        
        depth = 1
        q = collections.deque([root])
        while q:
            for _ in range(len(q)):
                node = q.popleft()
                if not node.left and not node.right:
                    return depth
            
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            depth += 1
        return depth
```