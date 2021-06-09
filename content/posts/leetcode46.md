---
title: "46 全排列"
date: 2021-06-08T22:06:15+08:00
draft: false
tags: ["算法", "回溯"]
categories: ["技术"]
---

**题目**

给定一个不含重复数字的数组 nums ，返回其 所有可能的全排列 。你可以 按任意顺序 返回答案。

示例 1：
```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

链接：https://leetcode-cn.com/problems/permutations


**解题思路**

```python
class Solution(object):
    def permute(self, nums):
        def helper(nums, path, ans):

            if len(path) == len(nums):
                ans.append(path[:])
                return
            
            for i in range(len(nums)):
                if nums[i] in path:
                    continue
                path.append(nums[i])
                helper(nums, path, ans)
                path.remove(nums[i])
        ans = []
        path = []
        helper(nums, path, ans)
        return ans
```
