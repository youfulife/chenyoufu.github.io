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
* [77 组合](https://leetcode-cn.com/problems/combinations)
* [78 子集](https://leetcode-cn.com/problems/subsets)
* [90 子集 II](https://leetcode-cn.com/problems/subsets-ii)
* [39 组合总和](https://leetcode-cn.com/problems/combination-sum)
* [40 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii)


### 总结

```python
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

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

**解题思路**

考虑重复元素一定要优先排序，将重复的都放在一起，便于找到重复元素和剪枝。如果前一个重复元素没有使用过，那么在当前重复元素下一层的可选项中一定会存在。

要解决重复问题，我们只要设定一个规则，保证在填第 \textit{idx}idx 个数的时候重复数字只会被填入一次即可。而在本题解中，我们选择对原数组排序，保证相同的数字都相邻，然后每次填入的数一定是这个数所在重复数集合中「从左往右第一个未被填过的数字」。


剪枝条件

剪枝条件1：用过的元素不能再使用之外，又添加了一个新的剪枝条件，也就是我们考虑重复部分思考的结果。

剪枝条件2：当当前元素和前一个元素值相同（此处隐含这个元素的 index>0 ），并且前一个元素还没有被使用过的时候，我们要剪枝。


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

### 77 组合

给定两个整数 n 和 k，返回范围 [1, n] 中所有可能的 k 个数的组合。

你可以按 任何顺序 返回答案。

**解题思路**

```python
class Solution(object):
    def combine(self, n, k):
        def dfs(depth, path, ans, start):

            if depth == k:
                ans.append(path[:])
                return
            
            for i in range(start, n+1):
                path.append(i)
                dfs(depth+1, path, ans, i+1)
                path.pop()
        
        ans = []
        dfs(0, [], ans, 1)
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

自己第一遍写的，东拼西凑的条件。。。

```python
class Solution(object):
    def subsets(self, nums):

        def dfs(nums, start, depth, path, ans):
            ans.append(path[:])
            if depth == len(nums):
                return
            
            for i in range(start, len(nums)):
                if len(path)>0 and nums[i] <= path[-1]:
                    continue
                path.append(nums[i])
                dfs(nums, start+1, depth+1, path, ans)
                path.pop()

        ans =[]
        nums.sort()
        dfs(nums, 0, 0, [], ans)
        return ans
```

优化

```python
class Solution(object):
    def subsets(self, nums):

        def dfs(nums, start, path, ans):
            ans.append(path[:])
            for i in range(start, len(nums)):
                path.append(nums[i])
                dfs(nums, i+1, path, ans)
                path.pop()

        ans =[]
        nums.sort()
        dfs(nums, 0, [], ans)
        return ans
```


### 90 子集 II

给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。返回的解集中，子集可以按 任意顺序 排列。

示例 1：
```
输入：nums = [1,2,2]
输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]
```

```python
class Solution(object):
    def subsetsWithDup(self, nums):
        def dfs(nums, start, path, ans, used):
            ans.append(path[:])

            for i in range(start, len(nums)):
                # 去重
                if i > 0 and nums[i] == nums[i-1] and used[i-1] == 0:
                    continue
                path.append(nums[i])
                used[i] = 1
                # 注意是i+1，不是start+1
                dfs(nums, i+1, path, ans, used)
                path.pop()
                used[i] = 0
        ans = []
        used = [0] * len(nums)
        # 不要忘记排序
        nums.sort()
        dfs(nums, 0, [], ans, used)
        return ans
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

```python
class Solution(object):
    def combinationSum(self, candidates, target):
        def dfs(nums, start, path, ans, target):
            if target < 0:
                return
            
            if target == 0:
                ans.append(path[:])
                return
            for i in range(start, len(nums)):
                path.append(nums[i])
                dfs(nums, i, path, ans, target-nums[i])
                path.pop()
        
        ans = []
        dfs(candidates, 0, [], ans, target)
        return ans
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

```python
class Solution(object):
    def combinationSum2(self, candidates, target):
        def dfs(nums, target, start, path, ans, used):
            if target < 0:
                return
            
            if target == 0:
                ans.append(path[:])
                return
            
            for i in range(start, len(nums)):
                if i > 0 and nums[i] == nums[i-1] and used[i-1] == 0:
                    continue
                used[i] = 1
                path.append(nums[i])
                dfs(nums, target-nums[i], i+1, path, ans, used)
                used[i] = 0
                path.pop()
        
        ans = []
        used = [0] * len(candidates)
        candidates.sort()
        dfs(candidates, target, 0, [], ans, used)
        return ans
```

### 参考

https://leetcode.com/problems/permutations/discuss/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning)