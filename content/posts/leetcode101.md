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