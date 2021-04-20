---
title: "226 翻转二叉树"
date: 2021-04-20T21:28:14+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

翻转一棵二叉树。

**示例：**

输入：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

**备注:**
这个问题是受到 Max Howell 的 原问题 启发的 ：

谷歌：我们90％的工程师使用您编写的软件(Homebrew)，但是您却无法在面试时在白板上写出翻转二叉树这道题，这太糟糕了。

链接：https://leetcode-cn.com/problems/invert-binary-tree

**解题思路**

V1 递归，自己写的

```python
class Solution(object):
    def invertTree(self, root):
        if root:
            self.helper(root)
        return root

    def helper(self, root):
        if not root.left and not root.right:
            return
        root.left, root.right = root.right, root.left
        if root.left:
            self.helper(root.left)
        if root.right:
            self.helper(root.right)
```

一开始做这个题错了3次才提交通过。

第一次是直接在invertTree中递归调用，没有返回值。

第二次是没有判断root是否为空。

第三次是helper中没有判断root.left和root.right是否为空。

**V2 官方题解**

这是一道很经典的二叉树问题。显然，我们从根节点开始，递归地对树进行遍历，并从叶子结点先开始翻转。如果当前遍历到的节点 root 的左右两棵子树都已经翻转，那么我们只需要交换两棵子树的位置，即可完成以 root 为根节点的整棵子树的翻转。

```python
class Solution:
    def invertTree(self, root: TreeNode) -> TreeNode:
        if not root:
            return root
        
        left = self.invertTree(root.left)
        right = self.invertTree(root.right)
        root.left, root.right = right, left
        return root
```

**V3 非递归 BFS**

```python
class Solution(object):
    def invertTree(self, root):
        if not root:
            return root
        
        q = deque([root])
        
        while q:
            node = q.popleft()
            node.left, node.right = node.right, node.left
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
        return root
```

**V4 非递归 DFS**

```python
class Solution(object):
    def invertTree(self, root):
        if not root:
            return root
        
        q = deque([root])
        
        while q:
            node = q.pop()
            node.left, node.right = node.right, node.left
            if node.left:
                q.append(node.left)
            if node.right:
                q.append(node.right)
        return root
```

