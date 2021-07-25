---
title: "904 水果成篮"
date: 2021-07-25T10:14:48+08:00
draft: false
tags: ["算法", "滑动窗口"]
categories: ["技术"]
---

**题目**

在一排树中，第 i 棵树产生 tree[i] 型的水果。
你可以从你选择的任何树开始，然后重复执行以下步骤：

把这棵树上的水果放进你的篮子里。如果你做不到，就停下来。
移动到当前树右侧的下一棵树。如果右边没有树，就停下来。
请注意，在选择一颗树后，你没有任何选择：你必须执行步骤 1，然后执行步骤 2，然后返回步骤 1，然后执行步骤 2，依此类推，直至停止。

你有两个篮子，每个篮子可以携带任何数量的水果，但你希望每个篮子只携带一种类型的水果。

用这个程序你能收集的水果树的最大总量是多少？

示例 1：
```
输入：[1,2,1]
输出：3
解释：我们可以收集 [1,2,1]。
```

链接：https://leetcode-cn.com/problems/fruit-into-baskets

**解题思路**

问题题等价于，找到最长的子序列，最多含有两种“类型”（tree[i] 的值）

```python
class Solution(object):
    def totalFruit(self, nums):
        left, right = 0, 0
        window = {}
        valid = 0
        k = 2
        ans = -1
        while right < len(nums):
            c = nums[right]
            right += 1
            window[c] = window.get(c, 0) + 1
            if window[c] == 1:
                valid += 1
            while valid > k:
                d = nums[left]
                left += 1
                if window[d] == 1:
                    valid -= 1
                window[d] -= 1
            if valid <= k:
                ans = max(ans, right - left)
        return ans
```