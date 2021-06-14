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

**字典序算法**

```python
class Solution(object):
    def permute(self, s):
        def next(s):
            n = len(s)

            # 第一个比右边小的数
            i = n - 2
            while i >= 0 and s[i] > s[i+1]:
                i -= 1
            
            if i == -1:
                return s, False
            
            # i右侧比i小的最小值
            j = i + 1
            while j < n:
                if s[j] > s[i]:
                    j += 1
                else:
                    break
            j -= 1
            # 交换
            s[i], s[j] = s[j], s[i]
            # i 后面逆序
            left = i + 1
            right = n-1
            while left < right:
                s[left], s[right] = s[right], s[left]
                left+=1
                right-=1
            
            return s, True

        p = sorted(s)
        ans = [p[::]]
        
        while True:
            p, ok = next(p)
            if not ok:
                break
            ans.append(p[::])
        return ans
```