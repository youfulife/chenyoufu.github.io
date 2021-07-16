---
title: "80 删除有序数组中的重复项 II"
date: 2021-07-14T23:26:00+08:00
draft: true
tags: ["算法", "数组", "双指针"]
categories: ["技术"]

---

**题目**
给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 最多出现两次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii

**解题思路**

```python
class Solution(object):
    def removeDuplicates(self, nums):
        j = 2
        for i in range(2, len(nums)):
            if nums[j-2] != nums[i]: # 注意这里是判断 j-2
                nums[j] = nums[i]
                j += 1
        return j
```

**通用解法**
为了让解法更具有一般性，我们将原问题的「保留 2 位」修改为「保留 k 位」。

对于此类问题，我们应该进行如下考虑：

* 由于是保留 k 个相同数字，对于前 k 个数字，我们可以直接保留
* 对于后面的任意数字，能够保留的前提是：与当前写入的位置前面的第 k 个元素进行比较，不相同则保留

```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        def solve(k):
            u = 0
            for x in nums:
                if u < k or nums[u - k] != x:
                    nums[u] = x
                    u += 1
            return u
        return solve(2)
```

**参考**


链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/solution/gong-shui-san-xie-guan-yu-shan-chu-you-x-glnq/
