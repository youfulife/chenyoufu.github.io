---
title: "101 对称二叉树"
date: 2021-04-17T22:47:50+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

**题目**

给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。
```

    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:
```
    1
   / \
  2   2
   \   \
   3    3
```

链接：https://leetcode-cn.com/problems/symmetric-tree

**解题思路**

如果同时满足下面的条件，两个树互为镜像：
* 它们的两个根结点具有相同的值
* 每个树的右子树都与另一个树的左子树镜像对称

```python
class Solution(object):
    def isSymmetric(self, root):
        if not root:
            return True
        
        return self.isSameTree(root.left, root.right)

    def isSameTree(self, p, q):
        if not p or not q:
            return p == q
        
        if p.val != q.val:
            return False
        
        return self.isSameTree(p.left, q.right) and self.isSameTree(p.right, q.left)
```

----

2021.05.06 二刷

```python
class Solution(object):
    def isSymmetric(self, root):
        if not root:
            return True
        def helper(p, q):
            if not p or not q:
                return p == q
            if p.val != q.val:
                return False
            return helper(p.left, q.right) and helper(p.right, q.left)
        return helper(root.left, root.right)
```

----

2021.07.03 三刷

**非递归 BFS**
```python
class Solution(object):
    def isSymmetric(self, root):
        if not root:
            return True
        
        q = deque([root])
        while q:
            level = []
            for _ in range(len(q)):
                node = q.popleft()
                level.append(node.val if node else None)
                if node: # 空节点也需要加上
                    q.append(node.right)
                    q.append(node.left)
            # 判断当前数组是否对称
            left = 0
            right = len(level) - 1
            while left < right:
                if level[left] != level[right]:
                    return False
                left += 1
                right -= 1
        return True
```

**非递归 DFS**

```python
class Solution(object):
    def isSymmetric(self, root):
        if not root:
            return True
        
        stack = [(root.left, root.right)]
        
        while stack:
            left, right = stack.pop()
            if not left or not right:
                if left != right:
                    return False
                else:
                    continue
            
            if left.val != right.val:
                return False
            stack.append((left.left, right.right))
            stack.append((left.right, right.left))
            
        return True
```

优化一下代码，一开始将两个root放入到stack中

```python
class Solution(object):
    def isSymmetric(self, root):
        stack = [(root, root)]
        while stack:
            left, right = stack.pop()
            if not left and not right:
                continue
            if not left or not right:
                return False
            if left.val != right.val:
                return False
            stack.append((left.left, right.right))
            stack.append((left.right, right.left))
        return True
```