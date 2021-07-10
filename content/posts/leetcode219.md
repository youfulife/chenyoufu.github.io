---
title: "219 存在重复元素 II"
date: 2021-07-10T10:03:18+08:00
draft: false
tags: ["算法", "字符串", "滑动窗口", "哈希表"]
categories: ["技术"]
---
**题目**

给定一个整数数组和一个整数 k，判断数组中是否存在两个不同的索引 i 和 j，使得 nums [i] = nums [j]，并且 i 和 j 的差的 绝对值 至多为 k。

示例 1:
```
输入: nums = [1,2,3,1], k = 3
输出: true
```
示例 2:
```
输入: nums = [1,0,1,1], k = 1
输出: true
```
示例 3:
```
输入: nums = [1,2,3,1,2,3], k = 2
输出: false
```

链接：https://leetcode-cn.com/problems/contains-duplicate-ii

**解题思路**

* 遍历数组所有元素，将下标数组存在一个哈希表中
* 遍历哈希表，针对重复的元素，判断是否存在距离小于等于k

```python
class Solution(object):
    def containsNearbyDuplicate(self, nums, k):
        d = {}
        for i, v in enumerate(nums):
            d[v] = d.get(v, []) + [i]
        for x in d:
            v = d[x]
            if len(v) > 1:
                for i in range(1, len(v)):
                    if v[i] - v[i-1] <= k:
                        return True
        return False
```

优化下，两个哈希表，其中一个存重复元素的最小距离

```python
class Solution(object):
    def containsNearbyDuplicate(self, nums, k):
        d = {}
        m = {}
        for i, v in enumerate(nums):
            if v in d:
                m[v] = min(m.get(v, i-d[v]), i - d[v])
            d[v] = i
        for x in m:
            if m[x] <= k:
                return True
        return False
```

继续优化, 感觉自己的前两个方法跟傻了一样，其实只要判断重复的过程中，直接再加一个判断条件判断距离就行了，hash表的值保存最近的下标。

```python
class Solution(object):
    def containsNearbyDuplicate(self, nums, k):
        d = {}
        for i, v in enumerate(nums):
            if v in d and i - d[v] <= k:
                return True
            d[v] = i
        return False
```

**滑动窗口**

```python
class Solution(object):
    def containsNearbyDuplicate(self, nums, k):
        window = {}
        left = 0
        right = 0
        while right < len(nums):
            c = nums[right]
            if c in window:
                return True
            window[c] = 1

            while len(window) > k:
                del window[nums[left]]
                left += 1
            
            right += 1
        return False
```