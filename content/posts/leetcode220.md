---
title: "220 存在重复元素 III"
date: 2021-07-11T21:51:15+08:00
draft: false
tags: ["算法", "数组", "滑动窗口"]
categories: ["技术"]
---

**题目**

给你一个整数数组 nums 和两个整数 k 和 t 。请你判断是否存在 两个不同下标 i 和 j，使得 abs(nums[i] - nums[j]) <= t ，同时又满足 abs(i - j) <= k 。

如果存在则返回 true，不存在返回 false。

示例 1：
```
输入：nums = [1,2,3,1], k = 3, t = 0
输出：true
```

链接：https://leetcode-cn.com/problems/contains-duplicate-iii

**解题思路**

朴素穷举，超时

```python
class Solution(object):
    def containsNearbyAlmostDuplicate(self, nums, k, t):
        n = len(nums)
        for i in range(n):
            for j in range(i+1, n):
                if abs(nums[i] - nums[j]) <= t and abs(i-j) <= k:
                    return True
        return False
```

优化后还是超时

```python
class Solution(object):
    def containsNearbyAlmostDuplicate(self, nums, k, t):
        n = len(nums)
        if n <= k + 1:
            return self.isOk(nums, t)
        for i in range(n - k):
            if self.isOk(nums[i: i+k+1], t):
                return True
        return False
    
    def isOk(self, nums, t):
        l = sorted(nums)
        for i in range(1, len(l)):
            if abs(l[i] - l[i-1]) <= t:
                return True
        return False
```

**滑动窗口+桶排序**


```python
class Solution(object):
    def containsNearbyAlmostDuplicate(self, nums, k, t):
        m = {}

        for i in range(len(nums)):
            # 维持窗口大小为k，注意边界
            if len(m) > k:
                id = self.getId(nums[i-k-1], t)
                del m[id]
            # 当前元素的桶的id，[0, t]为一个0号桶，[t+1, 2t+1] 为1号桶...
            x = nums[i]
            id = self.getId(x, t)
            if id in m:
                return True
            if id-1 in m and x - m[id-1] <= t:
                return True
            if id+1 in m and m[id+1] - x <= t:
                return True
            m[id] = x
            
        return False
    
    # 因为这里用的python，所以不需要额外处理负数，如果是其他语言比如c/java等，需要特殊处理负数
    # https://stackoverflow.com/questions/19517868/integer-division-by-negative-number
    def getId(self, n, t):
        return n / (t + 1)
```

**参考**

* https://leetcode-cn.com/problems/contains-duplicate-iii/solution/gong-shui-san-xie-yi-ti-shuang-jie-hua-d-dlnv/
* https://leetcode-cn.com/problems/contains-duplicate-iii/solution/c-li-yong-tong-fen-zu-xiang-xi-jie-shi-b-ofj6/

