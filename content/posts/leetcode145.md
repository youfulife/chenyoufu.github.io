---
title: "145 二叉树的后序遍历"
date: 2021-06-13T21:45:11+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

**解题思路**

```python
class Solution(object):
    def postorderTraversal(self, root):
        if not root:
            return []
        ans = []
        self.traversal(root, ans)
        return ans
    
    def traversal(self, root, ans):
        if not root:
            return None
        self.traversal(root.left, ans)
        self.traversal(root.right, ans)
        ans.append(root.val)
```

**非递归V1**

思路

* 先序遍历顺序：根节点-左孩子-右孩子
* 后序遍历顺序：左孩子-右孩子-根节点
* 后序遍历倒过来：根节点-右孩子-左孩子

第一步，将二叉树按照先序非递归算法进行遍历，注意在入栈的时候左右孩子入栈的顺序，先左后右。

第二步，将遍历得到的结果进行倒置。 

这个思路实际上还是很巧妙的，值得学习。

```python
class Solution(object):
    def postorderTraversal(self, root):
        if not root:
           return []
        
        ans = []
        stack = [root]
        while stack:
            node = stack.pop()
            ans.append(node.val)
            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)
        return ans[::-1]
```

**非递归V2 染色法**

```python
class Solution(object):
    def postorderTraversal(self, root):
        if not root:
           return []

        WHITE, GRAY = 0, 1
        
        ans = []
        stack = [(WHITE, root)]
        while stack:
            color, node = stack.pop()
            if not node:
                continue
            if color == WHITE:
                stack.append((GRAY, node))
                stack.append((WHITE, node.right))
                stack.append((WHITE, node.left))
            else:
                ans.append(node.val)
        return ans
```