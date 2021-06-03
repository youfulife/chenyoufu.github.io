---
title: "剑指 Offer 07. 重建二叉树"
date: 2021-06-01T22:12:19+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

**题目**

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]

返回如下的二叉树：

    3
   / \
  9  20
    /  \
   15   7
 
```

链接：https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof

**解题思路**

```go
func buildTree(preorder []int, inorder []int) *TreeNode {
    
    if len(preorder) == 0 || len(inorder) == 0 {
        return nil
    }
    
    root := &TreeNode{
        Val: preorder[0],
    }

    i:=0
    for ; i < len(inorder); i++ {
        if preorder[0] == inorder[i] {
            break
        }
    }
    leftIn := inorder[:i]
    rightIn := inorder[i+1:]

    root.Left = buildTree(preorder[1:len(leftIn)+1], leftIn)
    root.Right = buildTree(preorder[len(leftIn)+1:], rightIn)
    return root
}
```