---
title: "Leetcode215"
date: 2021-06-06T14:23:30+08:00
draft: false
tags: ["算法", "数组"]
categories: ["技术"]
---

**题目**

在未排序的数组中找到第 k 个最大的元素。请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

示例 1:
```
输入: [3,2,1,5,6,4] 和 k = 2
输出: 5
```
示例 2:
```
输入: [3,2,3,1,2,4,5,5,6] 和 k = 4
输出: 4
```
说明:

你可以假设 k 总是有效的，且 1 ≤ k ≤ 数组的长度。

链接：https://leetcode-cn.com/problems/kth-largest-element-in-an-array

**解题思路**

从大到小排序，找到第k个元素。

```go
func findKthLargest(nums []int, k int) int {

    for i:= 0; i < k; i++ {
        for j := i+1; j < len(nums); j++ {
            if nums[j] > nums[i] {
                nums[i], nums[j] = nums[j], nums[i]
            }
        }
    }

    return nums[k-1]
}
```

**基于快速排序**

```go
func findKthLargest(nums []int, k int) int {

    left := 0
    right := len(nums)-1

    return quickSort(nums, left, right, k)   
}

func quickSort(nums []int, left, right, k int) int {

    if left >= right {
        return nums[right]
    }

    pivot := nums[left]

    i := left
    j := right

    for i < j {
        for i < j && nums[j] >= pivot {
            j--
        }
        nums[i] = nums[j]

        for i < j && nums[i] <= pivot {
            i++
        } 
        nums[j] = nums[i]
    }
    nums[i] = pivot

    if i > len(nums) - k {
        return quickSort(nums, left, i-1, k)
    } else if i < len(nums) - k {
        return quickSort(nums, i+1, right, k)
    }

    return pivot
}
```

**基于堆排序**

https://github.com/python/cpython/blob/main/Lib/heapq.py

```python
class Solution(object):
    def findKthLargest(self, nums, k):
        if k > len(nums):
            return -1
        
        l = []
        for i in range(k):
            heappush(l, nums[i])
        
        for i in range(k, len(nums)):
            if nums[i] > l[0]:
                heappop(l)
                heappush(l, nums[i])
        
        return l[0]
```