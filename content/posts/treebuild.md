---
title: "构造二叉树"
date: 2021-09-04T23:19:37+08:00
draft: false
tags: ["算法", "树", "DFS", "五星"]
categories: ["技术"]
---

**题目**

* 105. 从前序与中序遍历序列构造二叉树
* 106. 从中序与后序遍历序列构造二叉树
* 889. 根据前序和后序遍历构造二叉树
* 1008. 前序遍历构造二叉搜索树

**解题思路**

### 106 从中序与后序遍历序列构造二叉树

* 前序和中序可以唯一确定一颗二叉树。
* 后序和中序可以唯一确定一颗二叉树。
* 前序和后序是不能唯一确定一颗二叉树的。因为没有中序遍历无法确定左右部分，也就是无法分割。

```python
class Solution(object):
    def buildTree(self, inorder, postorder):
        if not inorder:
            return None
        n = len(inorder)
        
        m = {inorder[i]: i for i in range(n)}
        
        def build(lefti, righti, leftp, rightp):
            if lefti > righti:
                return None
            
            root = TreeNode(postorder[rightp])

            root_i = m[root.val]
            left_num = root_i - lefti
            right_num = righti - root_i

            root.left = build(lefti, root_i - 1 , leftp, leftp + left_num-1)
            root.right = build(root_i + 1, righti, rightp - right_num, rightp-1)
            return root

        
        return build(0, n-1, 0, n-1)
```

分割数组

```python
class Solution(object):
    def buildTree(self, inorder, postorder):
        if not inorder:
            return None
                        
        root = TreeNode(postorder[-1])
        i = inorder.index(root.val)

        root.left = self.buildTree(inorder[:i], postorder[:i])
        root.right = self.buildTree(inorder[i+1:], postorder[i: -1])
        return root
```

#### 1008. 前序遍历构造二叉搜索树

```python
class Solution(object):
    def bstFromPreorder(self, preorder):
        if not preorder:
            return None
        
        root = TreeNode(preorder[0])

        # 可以优化为二分查找
        i = 1
        while i < len(preorder) and preorder[i] < root.val:
            i += 1
        
        left = preorder[1:i]
        right = preorder[i:]

        root.left = self.bstFromPreorder(left)
        root.right = self.bstFromPreorder(right)
        return root
```



参考：

https://leetcode-cn.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/solution/tu-jie-gou-zao-er-cha-shu-wei-wan-dai-xu-by-user72/


构造二叉树专题

https://lucifer.ren/blog/2020/02/08/%E6%9E%84%E9%80%A0%E4%BA%8C%E5%8F%89%E6%A0%91%E4%B8%93%E9%A2%98/