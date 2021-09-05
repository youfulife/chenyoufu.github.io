---
title: "105 从前序与中序遍历序列构造二叉树"
date: 2021-09-04T17:23:09+08:00
draft: false
tags: ["算法", "树", "DFS", "五星"]
categories: ["技术"]
---

**题目**

给定一棵树的前序遍历 preorder 与中序遍历  inorder。请构造二叉树并返回其根节点。

```
Input: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
Output: [3,9,20,null,null,15,7]
```

链接：https://leetcode-cn.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal

**解题思路**

自己没想出来。看官方题解后可以自己写出来代码。

对于任意一颗树而言，前序遍历的形式总是 [ 根节点, [左子树的前序遍历结果], [右子树的前序遍历结果] ]
即根节点总是前序遍历中的第一个节点。

而中序遍历的形式总是 [ [左子树的中序遍历结果], 根节点, [右子树的中序遍历结果] ]

只要我们在中序遍历中定位到根节点，那么我们就可以分别知道左子树和右子树中的节点数目。由于同一颗子树的前序遍历和中序遍历的长度显然是相同的，因此我们就可以对应到前序遍历的结果中，对上述形式中的所有左右括号进行定位。

这样以来，我们就知道了左子树的前序遍历和中序遍历结果，以及右子树的前序遍历和中序遍历结果，我们就可以递归地对构造出左子树和右子树，再将这两颗子树接到根节点的左右位置。

```python
class Solution(object):
    def buildTree(self, preorder, inorder):
        if not preorder:
            return None

        n = len(preorder)

        root = TreeNode(preorder[0])
        rpos = 0
        for i in range(n):
            if inorder[i] == root.val:
                rpos = i
                break
        lefti = inorder[:rpos]
        righti = inorder[rpos+1:]
        leftp = preorder[1: len(lefti)+1]
        rightp = preorder[1+len(lefti):]

        root.left = self.buildTree(leftp, lefti)
        root.right = self.buildTree(rightp, righti)
        return root
```