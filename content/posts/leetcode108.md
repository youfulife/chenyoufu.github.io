---
title: "108 将有序数组转换为二叉搜索树"
date: 2021-05-12T22:34:24+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

**题目**

给你一个整数数组 nums ，其中元素已经按 升序 排列，请你将其转换为一棵 高度平衡 二叉搜索树。

高度平衡 二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1 」的二叉树。

![](/img/btree1.jpg)

```
输入：nums = [-10,-3,0,5,9]
输出：[0,-3,9,-10,null,5]
解释：[0,-10,5,null,-3,null,9] 也将被视为正确答案：
```

链接：https://leetcode-cn.com/problems/convert-sorted-array-to-binary-search-tree

**解题思路**

```python
class Solution(object):
    def sortedArrayToBST(self, nums):
        
        def helper(nums, left, right):
            
            if left > right:
                return None
            
            mid = (left + right) / 2
            root = TreeNode(nums[mid])
            
            root.left = helper(nums, left, mid-1)
            root.right = helper(nums, mid+1, right)
            return root
        
        return helper(nums, 0, len(nums) - 1)
```

简单优化后：

```python
class Solution(object):
    def sortedArrayToBST(self, nums):
        if not nums:
            return None
        
        mid = len(nums)/2
        
        root = TreeNode(nums[mid])
        
        root.left = self.sortedArrayToBST(nums[:mid])
        root.right = self.sortedArrayToBST(nums[mid+1:])
        
        return root
```