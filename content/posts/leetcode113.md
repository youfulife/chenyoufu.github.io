---
title: "113 路径总和II"
date: 2021-04-21T22:10:42+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---


给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。

叶子节点 是指没有子节点的节点。

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]
```

链接：https://leetcode-cn.com/problems/path-sum-ii

**解题思路**

和112的题目类似, 只不过要返回所有的满足条件的路径。这个题目和字符串的path相比不一样的是，通过数组返回。

在写的过程中注意python的数组是引用，所以每次需要对path重新赋值，而不能在原先的path上操作。

```python
class Solution(object):
    def pathSum(self, root, targetSum):
        if not root:
            return []

        ans = []
        path = [root.val]
        self.helper(root, targetSum, path, ans)
        return ans
    
    def helper(self, root, targetSum, path, ans):
        if not root.left and not root.right and targetSum == root.val:
            ans.append(path) 
        
        if root.left:
            self.helper(root.left, targetSum-root.val, path+[root.left.val], ans)
        if root.right:
            self.helper(root.right, targetSum-root.val, path+[root.right.val], ans)
```
