---
title: "回溯 排列组合题"
date: 2021-09-14T22:06:15+08:00
draft: false
tags: ["算法", "回溯", "DFS", "排列组合"]
categories: ["技术"]
---

### 题目

* [46 全排列](https://leetcode-cn.com/problems/permutations)
* [47 全排列 II](https://leetcode-cn.com/problems/permutations-ii)
* [78 子集](https://leetcode-cn.com/problems/subsets)
* [90 子集 II](https://leetcode-cn.com/problems/subsets-ii)
* [39 组合总和](https://leetcode-cn.com/problems/combination-sum)
* [40 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii)
* [131 分割回文串](https://leetcode-cn.com/problems/palindrome-partitioning)


### 总结

1. 辅助函数的名字叫dfs，参数分别为nums, depth, path, ans, 这样命名更容易记住，同时更符合回溯树的图形化


### 46 全排列

给定一个不含重复数字的数组 nums ，返回其 所有可能的全排列 。你可以 按任意顺序 返回答案。

示例 1：
```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**解题思路**

```python
class Solution(object):
    def permute(self, nums):
        def dfs(nums, depth, path, ans):
            if depth == len(nums):
                ans.append(path[:])
                return
            
            for x in nums:
                if x in path:
                    continue
                path.append(x)
                dfs(nums, depth+1, path, ans)
                path.pop()
        
        ans = []
        dfs(nums, 0, [], ans)
        return ans
```

### 47 全排列 II

给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

```python
class Solution(object):
    def permuteUnique(self, nums):
        def dfs(nums, depth, path, ans, used):
            if depth == len(nums):
                ans.append(path[:])
                return

            for i in range(len(nums)):
                # 去重
                if used[i] or (i > 0 and nums[i] == nums[i-1] and not used[i-1]):
                    continue
                path.append(nums[i])
                used[i] = True
                dfs(nums, depth+1, path, ans, used)
                path.pop()
                used[i] = False
        
        ans = []
        used = [False] * len(nums)
        nums.sort()
        dfs(nums, 0, [], ans, used)
        return ans
```

### 78 子集

给你一个整数数组 nums ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。

示例 1：
```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```


### 90 子集 II

给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。返回的解集中，子集可以按 任意顺序 排列。

示例 1：
```
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```


### 39 组合总和

给定一个无重复元素的正整数数组 candidates 和一个正整数 target ，找出 candidates 中所有可以使数字和为目标数 target 的唯一组合。

candidates 中的数字可以无限制重复被选取。如果至少一个所选数字数量不同，则两种组合是唯一的。 

对于给定的输入，保证和为 target 的唯一组合数少于 150 个。

示例 1：
```
输入: candidates = [2,3,6,7], target = 7
输出: [[7],[2,2,3]]
```

### 40 组合总和 II

给定一个数组 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用一次。

注意：解集不能包含重复的组合。 

示例 1:
```
输入: candidates = [10,1,2,7,6,1,5], target = 8,
输出:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]
```


### 131 分割回文串

给你一个字符串 s，请你将 s 分割成一些子串，使每个子串都是 回文串 。返回 s 所有可能的分割方案。

回文串 是正着读和反着读都一样的字符串。

示例 1：
```
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
```


### 参考

https://leetcode.com/problems/permutations/discuss/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning)