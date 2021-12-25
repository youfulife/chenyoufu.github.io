---
title: "回溯 排列组合题"
date: 2021-09-14T22:06:15+08:00
draft: false
tags: ["算法", "回溯", "DFS", "排列组合"]
categories: ["技术"]
---


2021.12.15 时隔3个月，二刷

### 解释递归后面状态重置是怎么回事
* 当回到上一级的时候，所有的状态变量需要重置为第一次来到该结点的状态，这样继续尝试新的选择才有意义；
* 在代码层面上，需要在递归结束以后，添加递归之前的操作的逆向操作；

### 基本类型变量和对象类型变量的不同处理
* 基本类型变量每一次向下传递的时候的行为是复制，所以无需重置；
* 对象类型变量在遍历的全程只有一份，因此再回退的时候需要重置；
* 类比于 Java 中的 方法参数 的传递机制：
* 基本类型变量在方法传递的过程中的行为是复制，每一次传递复制了参数的值；
* 对象类型变量在方法传递的过程中复制的是对象地址，对象全程在内存中共享地址。
### 字符串问题的特殊性
* 如果使用 + 拼接字符串，每一次拼接产生新的字符串，因此无需重置；
* 如果使用 StringBuilder 拼接字符串，整个搜索的过程 StringBuilder 对象只有一份，需要状态重置。
### 为什么不是广度优先遍历
* 广度优先遍历每一层需要保存所有的「状态」，如果状态空间很大，需要占用很大的内存空间；
* 深度优先遍历只要有路径可以走，就继续尝试走新的路径，不同状态的差距只有一个操作，而广度优先遍历在不同的层之前，状态差异很大，就不能像深度优先遍历一样，可以 使用一份状态变量去遍历所有的状态空间，在合适的时候记录状态的值就能得到一个问题的所有的解。


2021.12.25 更新

对于有重复元素的排练组合问题完全记不得思路了，比如题目 全排列 II。

要去重的是同一树层上的“使用过”，同一树枝上的都是一个组合里的元素，不用去重。

在candidates[i] == candidates[i - 1]相同的情况下：

* used[i - 1] == true，说明同一树支candidates[i - 1]使用过
* used[i - 1] == false，说明同一树层candidates[i - 1]使用过。（回溯的时候将used[i-1]从true又变回false了）


----
### 题目

* [46 全排列](https://leetcode-cn.com/problems/permutations)
* [47 全排列 II](https://leetcode-cn.com/problems/permutations-ii)
* [77 组合](https://leetcode-cn.com/problems/combinations)
* [78 子集](https://leetcode-cn.com/problems/subsets)
* [90 子集 II](https://leetcode-cn.com/problems/subsets-ii)
* [39 组合总和](https://leetcode-cn.com/problems/combination-sum)
* [40 组合总和 II](https://leetcode-cn.com/problems/combination-sum-ii)
* [17 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number)
* [1079 活字印刷](https://leetcode-cn.com/problems/letter-tile-possibilities/)

### 总结

for循环横向遍历，递归纵向遍历，回溯不断调整结果集，这个理念贯穿整个回溯法系列

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
2. 回溯问题抽象为树形结构，可以直观的看出其搜索的过程：for循环横向遍历，递归纵向遍历，回溯不断调整结果集。

回溯算法能解决如下问题：

* 组合问题：N个数里面按一定规则找出k个数的集合
* 排列问题：N个数按一定规则全排列，有几种排列方式
* 切割问题：一个字符串按一定规则有几种切割方式
* 子集问题：一个N个数的集合里有多少符合条件的子集
* 棋盘问题：N皇后，解数独等等

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


参考:

[组合总和II](https://programmercarl.com/0040.%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8CII.html#%E5%9B%9E%E6%BA%AF%E4%B8%89%E9%83%A8%E6%9B%B2)

本题的难点在于区别2中：集合（数组candidates）有重复元素，但还不能有重复的组合。

一些同学可能想了：我把所有组合求出来，再用set或者map去重，这么做很容易超时！

所以要在搜索的过程中就去掉重复组合。

很多同学在去重的问题上想不明白，其实很多题解也没有讲清楚，反正代码是能过的，感觉是那么回事，稀里糊涂的先把题目过了。

这个去重为什么很难理解呢，所谓去重，其实就是使用过的元素不能重复选取。 这么一说好像很简单！

都知道组合问题可以抽象为树形结构，那么“使用过”在这个树形结构上是有两个维度的，一个维度是同一树枝上使用过，一个维度是同一树层上使用过。没有理解这两个层面上的“使用过” 是造成大家没有彻底理解去重的根本原因。

回看一下题目，元素在同一个组合内是可以重复的，怎么重复都没事，但两个组合不能相同。

所以我们要去重的是同一树层上的“使用过”，同一树枝上的都是一个组合里的元素，不用去重。


在candidates[i] == candidates[i - 1]相同的情况下：

* used[i - 1] == true，说明同一树支candidates[i - 1]使用过
* used[i - 1] == false，说明同一树层candidates[i - 1]使用过。（回溯的时候将used[i-1]从true又变回false了）


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

```python
class Solution(object):
    def subsets(self, nums):
        path = []
        ans = []
        n = len(nums)
        def dfs(start):
            ans.append(path[:])
            for i in range(start, n):
                path.append(nums[i])
                dfs(i+1)
                path.pop()
        dfs(0)
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
        n = len(nums)
        path = []
        ans = []
        # 在子集的基础上处理重复, 排序 + 访问记录
        used = [0] * n
        nums.sort()
        
        def dfs(start):
            ans.append(path[:])
            for i in range(start, n):
                # 去重的是同一树层上的“使用过”
                if i > 0 and nums[i] == nums[i-1] and used[i-1] == 0:
                    continue
                # 不要忘记标记
                used[i] = 1
                path.append(nums[i])
                # 下一次的起始点 i+1
                dfs(i+1)
                used[i] = 0
                path.pop()
        dfs(0)
        return ans
```

### 1079 活字印刷

你有一套活字字模 tiles，其中每个字模上都刻有一个字母 tiles[i]。返回你可以印出的非空字母序列的数目。

注意：本题中，每个活字字模只能使用一次。

示例 1：
```
输入："AAB"
输出：8
解释：可能的序列为 "A", "B", "AA", "AB", "BA", "AAB", "ABA", "BAA"。
```

**解题思路**

参考：https://leetcode-cn.com/problems/letter-tile-possibilities/solution/java-hui-su-suan-fa-jie-jue-by-sdwwld-5r4r/

前面介绍的几个组合的回溯算法，因为结果不能有重复的（比如[1，3]和[3，1]被认为是重复的结果），所以每次选择的时候都只能从前往后选。但这题中子集[A，B]和[B，A]被认为是两种不同的结果，所以每次都要从头开始选择，因为每个字符只能被使用一次，所以如果使用之后下次就不能再使用了，这里可以使用一个数组visit来标记有没有被使用。

```python
class Solution(object):
    def numTilePossibilities(self, tiles):
        tiles = sorted(tiles)
        n = len(tiles)
        ans = []
        used = [0] * n

        def dfs(path):
            if path != "":
                ans.append(path)
            if len(path) == len(tiles):
                return
            # 注意，这里的i每次都是从0开始的，不是从start开始
            for i in range(n):
                # 一个字符只能选择一次，如果当前字符已经选择了，就不能再选了。
                if used[i] == 1:
                    continue
                # 过滤掉重复的结果
                if i > 0 and tiles[i] == tiles[i-1] and used[i-1] == 0:
                    continue
                used[i] = 1
                dfs(path + tiles[i])
                used[i] = 0
        dfs("")
        return len(ans)
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

### 17 电话号码的字母组合

给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。答案可以按 任意顺序 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

示例 1：
```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**解题思路**

本题开始用多个集合来求组合，还是熟悉的模板题目，但是有一些细节。

例如这里for循环，可不像是在 回溯算法：求组合问题！ (opens new window)和回溯算法：求组合总和！ (opens new window)中从startIndex开始遍历的。

因为本题每一个数字代表的是不同集合，也就是求不同集合之间的组合，而回溯算法：求组合问题！ (opens new window)和回溯算法：求组合总和！ (opens new window)都是是求同一个集合中的组合！

```python
class Solution(object):
    def letterCombinations(self, digits):
        n = len(digits)
        if n == 0:
            return []
        m = {
            "2": ['a', 'b', 'c'],
            "3": ['d', 'e', 'f'],
            "4": ['g', 'h', 'i'],
            "5": ['j', 'k', 'l'],
            "6": ['m', 'n', 'o'],
            "7": ['p', 'q', 'r', 's'],
            "8": ['t', 'u', 'v'],
            "9": ['w', 'x', 'y', 'z'],
        }

        arr = [x for x in digits]
        ans = []

        def dfs(arr, start, n, path):
            if len(path) == n:
                ans.append(path)
                return
            
            for x in m[arr[start]]:
                dfs(arr, start+1, n, path + x)

        dfs(arr, 0, n, "")
        return ans
```

### 参考

https://leetcode.com/problems/permutations/discuss/18239/A-general-approach-to-backtracking-questions-in-Java-(Subsets-Permutations-Combination-Sum-Palindrome-Partioning)

https://programmercarl.com/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E5%85%B6%E4%BB%96%E8%AF%AD%E8%A8%80%E7%89%88%E6%9C%AC