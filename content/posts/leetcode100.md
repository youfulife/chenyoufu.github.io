---
title: "100 相同的树"
date: 2021-04-11T22:21:29+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

**题目**

给你两棵二叉树的根节点 p 和 q ，编写一个函数来检验这两棵树是否相同。
如果两个树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

**提示：**

两棵树上的节点数目都在范围 [0, 100] 内
-104 <= Node.val <= 104

链接：https://leetcode-cn.com/problems/same-tree/

**解题思路**

先判断左右节点是否存在，然后判断节点的值，再递归判断左右子树是否相同。

V1 自己第一遍写的。

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */
func isSameTree(p *TreeNode, q *TreeNode) bool {

    if p == nil && q == nil {
        return true
    }

    if p != nil && q != nil {
        if p.Val == q.Val && isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right) {
            return true
        } else {
            return false
        }
    }

    return false
}
```

V2 看了官方题解后优化, 更加直观清晰。

```go
func isSameTree(p *TreeNode, q *TreeNode) bool {

    if p == nil && q == nil {
        return true
    }

    if p == nil || q == nil {
        return false
    }

    if p.Val != q.Val {
        return false
    }

    return  isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)
}
```

V3 国际站评论区继续简化代码

```go
func isSameTree(p *TreeNode, q *TreeNode) bool {
    if p == nil || q == nil {
        return p == q
    }
    
    return p.Val == q.Val && isSameTree(p.Left, q.Left) && isSameTree(p.Right, q.Right)
}
```