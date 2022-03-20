---
title: "排序"
date: 2021-05-29T11:21:01+08:00
draft: false
tags: ["算法", "排序", "再刷"]
categories: ["技术"]
---

https://leetcode-cn.com/problems/sort-an-array/

**选择排序**
最容易理解。

首先，找到数组中最小的那个元素，其次，将它和数组的第 一个元素交换位置(如果第一个元素就是最小元素那么它就和自己交换)。再次，在剩下的元素中 找到最小的元素，将它与数组的第二个元素交换位置。如此往复，直到将整个数组排序。这种方法 叫做选择排序，因为它在不断地选择剩余元素之中的最小者。

如算法 2.1 所示，选择排序的内循环只是在比较当前元素与目前已知的最小元素(以及将当前 索引加 1 和检查是否代码越界)，这已经简单到了极点。交换元素的代码写在内循环之外，每次交 换都能排定一个元素，因此交换的总次数是 N。所以算法的时间效率取决于比较的次数。

```golang
func selectSort(a []int) {

  l := len(arr)
  for i:=0; i<l; i++ {
    min = i
    for j:=i+1; j<l; j++ {
      if a[j] < a[min] {
        min = j
      }
    }
    a[i], a[min] = a[min], a[i]
  }

  return
}
```

**插入排序**

通常人们整理桥牌的方法是一张一张的来，将每一张牌插入到其他已经有序的牌中的适当位置。 在计算机的实现中，为了给要插入的元素腾出空间，我们需要将其余所有元素在插入之前都向右移 动一位。这种算法叫做插入排序，与选择排序一样，当前索引左边的所有元素都是有序的，但它们的最终位置还不确定，为了给 更小的元素腾出空间，它们可能会被移动。但是当索引到达数组的右端时，数组排序就完成了。

和选择排序不同的是，插入排序所需的时间取决于输入中元素的初始顺序。例如，对一个很大 且其中的元素已经有序(或接近有序)的数组进行排序将会比对随机顺序的数组或是逆序数组进行 排序要快得多。

插入排序不会访问索引 右侧的元素，而选择排序不会访问索引左侧的 元素。

```go

func insertSort(nums []int) {
  if nums == nil {
    return
  }

  for i:= 1; i < len(nums); i++ {
        for j:=i; j > 0 && nums[j] > nums[j-1]; j-- {
            nums[j], nums[j-1] = nums[j-1], nums[j]
        }
    }
  return
}
```

**冒泡排序**

```go

func bubbleSort(arr []int) {

  l := len(arr)
  for i:=0; i<l; i++ {
    for j:=0; j<l-1-i; j++ {
      flag := true
      if a[j] < a[j+1] {
        a[j], a[j+1] = a[j+1], a[j]
        flag = false
      }
    }
    // 假如从开始的第一对到结尾的最后一对，相邻的元素之间都没有发生交换的操作，
    // 这意味着右边的元素总是大于等于左边的元素，此时的数组已经是有序的了，
    // 我们无需再对剩余的元素重复比较下去了。
    if flag {
      break
    }
  }
  return
}
```

**快速排序**

```go
func quickSort(nums []int, left, right int) {

    if left >= right {
        return
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
    quickSort(nums, left, i-1)
    quickSort(nums, i+1, right)
}
```

**希尔排序**


**归并排序**


**堆排序**


-----

2021.7.7

选择排序

```python
class Solution(object):
    def sortArray(self, nums):
        n = len(nums)
        for i in range(n):
            minIndex = i
            for j in range(i, n):
                if nums[j] < nums[minIndex]:
                    minIndex = j
            nums[i], nums[minIndex] = nums[minIndex], nums[i]
        return nums
```


2021.7.8

冒泡排序

```python
class Solution(object):
    def sortArray(self, nums):
        n = len(nums)
        for i in range(n):
            for j in range(n-1-i):
                if nums[j+1] < nums[j]:
                    nums[j], nums[j+1] = nums[j+1], nums[j]
        return nums
```

快速排序

```python
class Solution(object):
    def sortArray(self, nums):
        def quickSort(nums, left, right):
            if left >= right:
                return 0
            i = partition(nums, left, right)
            quickSort(nums, left, i-1)
            quickSort(nums, i+1, right)

        def partition(nums, left, right):
            pivot = nums[left]
            i = left
            j = right
            while i < j:
                while i<j and nums[j]>=pivot:
                    j-=1
                nums[i] = nums[j]
                while i<j and nums[i]<=pivot:
                    i+=1
                nums[j] = nums[i]
            nums[i] = pivot
            return i
        quickSort(nums, 0, len(nums)-1)
        return nums

class Solution(object):
    def sortArray(self, nums):
        def quickSort(nums, left, right):
            if left >= right:
                return
            
            pivot = nums[left]
            i = left
            j = right
            while i < j:
                while i<j and nums[j]>= pivot:
                    j -= 1
                nums[i] = nums[j]
                while i<j and nums[i] <= pivot:
                    i += 1
                nums[j] = nums[i]
            nums[i] = pivot
            quickSort(nums, left, i-1)
            quickSort(nums, i+1, right)
        quickSort(nums, 0, len(nums)-1)
        return nums


class Solution(object):
    def sortArray(self, nums):
        def quickSort(nums, left, right):
            if left >= right:
                return
            
            pivot = nums[left]
            i = left + 1
            j = i
            while j <= right:
                if nums[j] < pivot:
                    nums[i], nums[j] = nums[j], nums[i]
                    i+=1
                j+=1
            nums[left], nums[i-1] = nums[i-1], nums[left]
            i -= 1
            quickSort(nums, left, i-1)
            quickSort(nums, i+1, right)
        quickSort(nums, 0, len(nums)-1)
        return nums


class Solution(object):
    def sortArray(self, nums):
        def quickSort(nums, left, right):
            if left >= right:
                return
            k = random.randint(left, right)
            nums[left], nums[k] = nums[k], nums[left]
            pivot = nums[left]
            i = left + 1
            j = left + 1
            while j <= right:
                if nums[j] < pivot:
                    nums[i], nums[j] = nums[j], nums[i]
                    i+=1
                j+=1
            nums[i-1], nums[left] = nums[left], nums[i-1]
            i-=1

            quickSort(nums, left, i-1)
            quickSort(nums, i+1, right)
        quickSort(nums, 0, len(nums)-1)
        return nums
```

插入排序

```python
class Solution(object):
    def sortArray(self, nums):
            n = len(nums)
            for i in range(1, n):
                temp = nums[i]
                j = i - 1
                while j>=0 and nums[j]>temp:
                    nums[j+1] = nums[j]
                    j -= 1
                nums[j+1] = temp
            return nums
```

归并排序

```python
def mergeSort(nums):
  temp = merge(nums, 0, len(nums)-1)
  for i in range(len(temp)):
    nums[i] = temp[i]
  return 

def merge(nums, left, right):
  if left >= right:
    return nums[left]
  
  mid = left + (right-left)/2
  arr1 = merge(nums, left, mid)
  arr2 = merge(nums, mid+1, right)

  temp = []
  i = 0
  j = 0
  while i < len(arr1) and j< len(arr2):
    if arr1[i] < arr2[j]:
      temp.append(arr1[i])
      i+=1
    else:
      temp.append(arr2[j])
      j+=1
  while i < len(arr1):
    temp.append(arr1[i])
    i+=1
  while j < len(arr2):
    temp.append(arr2)
    j+=1
  return temp
```

堆排序


-----

2021.7.22

快速排序

```python
def quickSort(nums):
  partition(nums, 0, len(nums))

def partition2(nums, left, right):
  if left >= right:
    return
  
  # 随机pivot
  x = random.randint(left, right-1)
  nums[left], nums[x] = nums[x], nums[left]

  pivot = nums[left]
  i = left + 1
  j = left + 1
  # [i, j)
  while i < right:
    if nums[i] < pivot:
      nums[i], nums[j] = nums[j], nums[i]
      j += 1
    i += 1

  nums[j-1], nums[left] = nums[left], nums[j-1]

  partition(nums, left, j-1)
  partition(nums, j, right)
  return

def partition3(nums, left, right):
  # 闭区间, 这里不能>=, ？ > 和 >= 应该都可以。
  if left > right:
    return
  
  # 随机pivot
  x = random.randint(left, right)
  nums[left], nums[x] = nums[x], nums[left]

  pivot = nums[left]
  i = left
  j = right
  # [i, j]
  while i < j:
    # nums[j] >= pivot 和 nums[i] <= pivot 至少有一个=，否则可能会死循环
    while i < j and nums[j] >= pivot:
      j -= 1
    nums[i] = nums[j]
    
    while i < j and nums[i] <= pivot:
      i += 1

    nums[j] = nums[i]
  nums[j] = pivot
  partition(nums, left, j-1)
  partition(nums, j+1, right)
  return
```

---

2021.11.27

快速排序3种方法，1种前后指针，2种左右指针。

这个左右指针法，和上面partition3 不太一样，这个更好理解。

partition可以认为是挖坑法。

```python
class Solution(object):

    def partition(self, nums, left, right):
        if left >= right:
            return

        x = random.randint(left, right)
        nums[left], nums[x] = nums[x], nums[left]
        
        pivot = nums[left]
        # i 如果等于left+1那么会出错
        i = left
        j = right
        while i < j:
            # 要先右再左，反过来会出错
            while i < j and  nums[j] > pivot:
                j -= 1
            # = 需要放在左边，放上面会出错
            while i < j and nums[i] <= pivot:
                i += 1
            nums[i],nums[j] = nums[j], nums[i]
        
        nums[i],nums[left] = nums[left], nums[i]
        self.partition(nums, left, j-1)
        self.partition(nums, j+1, right)
        return


    def sortArray(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        self.partition(nums, 0, len(nums)-1)
        return nums
```

-----

2022.03.20

注意对比和上面一种写法的异同点。

```python
class Solution(object):
    def sortArray(self, nums):
        def partition(nums, start, end):
            if start >= end:
                return
            k = random.randint(start, end)
            nums[start], nums[k] = nums[k], nums[start]
            
            pivot = start
            left = start + 1
            right = end
            while left < right:
                while left < right and nums[left] <= nums[pivot]:
                    left += 1
                while left < right and nums[right] > nums[pivot]:
                    right -= 1
                
                nums[left], nums[right] = nums[right], nums[left]
            # 因为先走的左，所以当left == right的时候，nums[left] <= nums[pivot]不会判断，需要外面再判断一下
            if nums[left] > nums[pivot]:
                left -= 1
            nums[left], nums[pivot] = nums[pivot], nums[left]
            partition(nums, start, left-1)
            partition(nums, left+1, end)
        
        partition(nums, 0, len(nums)-1)
        return nums
```