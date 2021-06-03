---
title: "26 删除有序数组中的重复项"
date: 2021-06-03T17:20:08+08:00
draft: false
tags: ["算法", "数组"]
categories: ["技术"]
---

**题目**

给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

**示例 1：**

```
输入：nums = [1,1,2]
输出：2, nums = [1,2]
解释：函数应该返回新的长度 2 ，并且原数组 nums 的前两个元素被修改为 1, 2 。不需要考虑数组中超出新长度后面的元素。
```

链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array

**解题思路**

双指针，定义两个指针 fast 和 slow 分别为快指针和慢指针，快指针表示遍历数组到达的下标位置，慢指针表示下一个不同元素要填入的下标位置，初始时两个指针都指向下标 1。


```python
class Solution(object):
    def removeDuplicates(self, nums):
        if len(nums) == 0: # 这个条件不能忘记
          return 0
        j = 1
        for i in range(1, len(nums)):
            if nums[i] == nums[i-1]:
                continue
            nums[j] = nums[i]
            j+=1
            
        return j
```

记录相同元素的个数，跟上面的方法类似，但是思路不同。

```python
class Solution(object):
    def removeDuplicates(self, nums):
        count = 0
        for i in range(1, len(nums)):
            if nums[i] == nums[i-1]:
                count += 1
            else:
                nums[i-count] = nums[i]
        return len(nums) - count
```

这个题目在隔了2个月后，直接做的时候耗时挺长的，来回在纸上比划差不多20分钟，如果是在面试过程中，肯定是挂掉了。