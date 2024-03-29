---
title: "Leetcode215"
date: 2021-06-06T14:23:30+08:00
draft: false
tags: ["算法", "数组", "快速排序", "排序"]
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

----

2021.11.27

快速排序

```python
class Solution(object):
    
    def findKthLargest(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: int
        """
        def partition(left, right):            
            x = random.randint(left, right)
            nums[left], nums[x] = nums[x], nums[left]

            pivot = nums[left]
            i = left
            j = right
            while i < j:
                while i < j and nums[j] > pivot:
                    j -= 1
                while i < j and nums[i] <= pivot:
                    i += 1
                nums[i], nums[j] = nums[j], nums[i]
            nums[i], nums[left] = nums[left], nums[i]
            if i == len(nums) - k:
                return pivot
            elif i < len(nums) - k:
                return partition(i+1, right)
            else:
                return partition(left, i-1)
        return partition(0, len(nums) - 1)        
```

----

2022.03.20

堆排序

```python
class Solution(object):
    def findKthLargest(self, nums, k):
        def heapify(arr, i, size):
            l = i * 2 + 1
            r = l + 1
            mini = i
            if l < size and arr[l] < arr[mini]:
                mini = l
            if r < size and arr[r] < arr[mini]:
                mini = r
            if mini != i:
                arr[i], arr[mini] = arr[mini], arr[i]
                heapify(arr, mini, size)

        def minHeap(arr):
            for i in range(k/2-1, -1, -1):
                heapify(arr, i, k)
        
        minHeap(nums)
        for i in range(k, len(nums)):
            if nums[i] > nums[0]:
                nums[0], nums[i] = nums[i], nums[0]
                minHeap(nums)
            
        return nums[0]
```