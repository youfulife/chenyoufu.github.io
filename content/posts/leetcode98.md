---
title: "98 验证二叉搜索树"
date: 2021-07-03T14:41:33+08:00
draft: false
tags: ["算法", "树", "二叉搜索树"]
categories: ["技术"]
---

**题目**

给定一个二叉树，判断其是否是一个有效的二叉搜索树。

假设一个二叉搜索树具有如下特征：

* 节点的左子树只包含小于当前节点的数。
* 节点的右子树只包含大于当前节点的数。
* 所有左子树和右子树自身必须也是二叉搜索树。

链接：https://leetcode-cn.com/problems/validate-binary-search-tree

**解题思路**

* 中序遍历所有节点的值，判断中序遍历的结果是否递增的。
* 注意如果存在相等的两个值也不算二叉搜索树。

```python
class Solution(object):
    def isValidBST(self, root):
        nums = []
        def midTraverse(root):
            if not root:
                return
            
            midTraverse(root.left)
            nums.append(root.val)
            midTraverse(root.right)
        midTraverse(root)
        for i in range(1, len(nums)):
            if nums[i] <= nums[i-1]:
                return False
        return True
```

**递归，设置一个上界和下届**

```python
class Solution(object):
    def isValidBST(self, root):
        def traverse(root, low, high):
            if not root:
                return True
            # 这里不能直接用 if low， 因为如果low的值为0，就无法执行当前分支，要明确判断是否为None
            if low is not None and root.val <= low:
                return False
            if high is not None and root.val >= high:
                return False
            
            if not traverse(root.left, low, root.val):
                return False
            if not traverse(root.right, root.val, high):
                return False
            return True

        return traverse(root, None, None)
```

**迭代，把递归转化为迭代来实现**

```python
class Solution(object):
    def isValidBST(self, root):
        if not root:
            return True
        
        stack = [(root, None, None)]
        while stack:
            node, low, high = stack.pop()
            if low is not None and node.val <= low:
                return False
            if high is not None and node.val >= high:
                return False

            if node.right:
                stack.append((node.right, node.val, high))        
            if node.left:
                stack.append((node.left, low, node.val))
        return True
```

**中序遍历非递归**

```python
class Solution(object):
    def isValidBST(self, root):
        if not root:
            return True
        
        last = float('-inf')
        stack = []
        while stack or root:
            while root:
                stack.append(root)
                root = root.left
            
            root = stack.pop()
            if root.val <= last:
                return False
            last = root.val
            root = root.right
        return True
```

**中序遍历非递归，染色法**

```python
class Solution(object):
    def isValidBST(self, root):
        WHITE, GRAY = 0, 1
        if not root:
            return True
        
        last = float('-inf')
        stack = [(root, WHITE)]
        while stack:
            node, color = stack.pop()
            if color == GRAY:
                if node.val <= last:
                    return False
                last = node.val
            else:
                if node.right:
                    stack.append((node.right, WHITE))
                stack.append((node, GRAY))
                if node.left:
                    stack.append((node.left, WHITE))
            
        return True
```