---
title: "35 搜索插入位置"
date: 2021-06-05T11:52:53+08:00
draft: false
tags: ["算法", "数组", "二分查找"]
categories: ["技术"]
---

**题目**

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

你可以假设数组中无重复元素。

**解题思路**

二分查找，提交了好几次才提交成功，各种边界的判断错误。说白了就是二分查找掌握的不熟练。

```python
class Solution(object):
    def searchInsert(self, nums, target):
        if not nums: # 一开始忘记写
            return 0

        left = 0
        right = len(nums)-1

        while left < right:
            mid = left + (right - left) / 2
            if nums[mid] > target:
                right = mid - 1
            elif nums[mid] < target:
                left = mid + 1
            else:
                return mid
        if nums[left] >= target: # 忘记=的情况，这样只有一个元素的时候就会报错
            return left # 一开始还写成left-1
        else:
            return left+1
```

其实标准的二分查找法就可以了，各种边界也可以不用特殊处理。

```python
class Solution(object):
    def searchInsert(self, nums, target):
        left = 0
        right = len(nums)-1

        while left <= right:
            mid = left + (right - left) / 2
            if nums[mid] > target:
                right = mid - 1
            elif nums[mid] < target:
                left = mid + 1
            else:
                return mid
        return left
```

python标准库的写法。搜索区间左闭右开[low, high) 返回做边界，最后返回left和right都是一样的。

为什么 left = mid + 1，right = mid ？和之前的算法不一样？

答：这个很好解释，因为我们的「搜索区间」是 [left, right) 左闭右开，所以当 nums[mid] 被检测之后，下一步的搜索区间应该去掉 mid 分割成两个区间，即 [left, mid) 或 [mid + 1, right)。

为什么该算法能够搜索左侧边界？

答：关键在于对于 nums[mid] == target 这种情况的处理：if (nums[mid] == target)
    right = mid;可见，找到 target 时不要立即返回，而是缩小「搜索区间」的上界 right，在区间 [left, mid) 中继续搜索，即不断向左收缩，达到锁定左侧边界的目的。


```python
class Solution(object):
    def searchInsert(self, nums, target):
        left = 0
        right = len(nums)

        while left < right:
            mid = left + (right - left) / 2
            if nums[mid] < target:
                left = mid + 1
            else:
                right = mid
        return left
```

**来自知乎匿名用户的总结**

不管怎么写，只要遵循一个原则，就是L是符合条件的的，R是不符合的（反过来也一样）。这个不变量将贯穿整个二分的过程。

什么意思呢？举个栗子，你需要在[l,r]区间中找到符合条件的最大的x，按照上面的原则，你的初始条件就应该是L=l, R=r+1, 注意这里的R是不符合条件的，因为它超出了边界，所以它不可能是答案。这时候，你的mid如果合法，就说明答案大于等于mid，所以应该更新L，如果mid不合法，直接把R设成mid。至于如何判断合法（是大于等于还是大于），该不该加一减一，按照这个原则都可以很容易想出来。

这样在整个二分的过程中，你会发现，R在任何时候都不可能是答案，那么这个循环的停止条件也很容易判断了，就是L+1=R，这时候说明L已经是符合条件的最大值了，因为再加一就是非法值。

