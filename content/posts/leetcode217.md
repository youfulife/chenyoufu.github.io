---
title: "217 存在重复元素"
date: 2021-05-31T19:20:43+08:00
draft: true
---
**题目**

给定一个整数数组，判断是否存在重复元素。

如果存在一值在数组中出现至少两次，函数返回 true 。如果数组中每个元素都不相同，则返回 false 。

```
示例 1:

输入: [1,2,3,1]
输出: true
```

链接：https://leetcode-cn.com/problems/contains-duplicate

**解题思路**

哈希存储出现次数

```python
class Solution(object):
    def containsDuplicate(self, nums):
        d = {}
        for n in nums:
            d[n] = d.get(n, 0) + 1
            if d[n] > 1:
                return True
        return False
```