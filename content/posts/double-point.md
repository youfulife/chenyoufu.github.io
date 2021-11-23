---
title: "双指针系列"
date: 2021-11-23T16:27:25+08:00
draft: false
tags: ["算法", "双指针"]
categories: ["技术"]
---




### 双指针解法 整体思想

一个指针 i 进行数组遍历，另外一个指针 j 指向有效数组的最后一个位置。

判断 i 所指向的值是否符合题目的要求，只有当 i 所指向的值符合要求（比如不想等，不等于某个特定的值等），才将 i 的值添加到 j 的下一位置。


### 26. 删除有序数组中的重复项 

给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array

**解题思路**

```python
class Solution(object):
    def removeDuplicates(self, nums):
        n = len(nums)
        if n == 0:
            return 0
        
        j = 0
        for i in range(n):
            if nums[i] != nums[j]:
                j += 1
                nums[j] = nums[i]
        return j + 1
```

解法二，通用处理思路

```python
class Solution(object):
    def removeDuplicates(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        
        j = 0
        for x in nums:
            if j < 1 or x != nums[j-1]:
                nums[j] = x
                j += 1
        return j
```

### 27. 移除元素

给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 O(1) 额外空间并 原地 修改输入数组。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

链接：https://leetcode-cn.com/problems/remove-element


**解题思路**

```python
class Solution(object):
    def removeElement(self, nums, val):
        j = 0
        for i in range(len(nums)):
            if nums[i] != val:
                nums[i], nums[j] = nums[j], nums[i]
                j += 1
        return j
```

### 283. 移动零

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。


**解题思路**

```python
class Solution(object):
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        j = 0
        for i in range(len(nums)):
            if nums[i] == 0:
                continue
            
            nums[i], nums[j] = nums[j], nums[i]
            j+=1
        
        return
```

不交换，最后设置为0。

```python
class Solution(object):
    def moveZeroes(self, nums):
        """
        :type nums: List[int]
        :rtype: None Do not return anything, modify nums in-place instead.
        """
        j = 0
        for i in range(len(nums)):
            if nums[i] == 0:
                continue
            
            nums[j] = nums[i]
            j+=1
        
        for i in range(j, len(nums)):
            nums[i] = 0
        
        return
```

### 80. 删除有序数组中的重复项 II

给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使每个元素 最多出现两次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii

**解题思路**

```python
class Solution(object):
    def removeDuplicates(self, nums):
        maxCnt = 2
        if len(nums) <= 2:
            return len(nums)
            
        j = 0
        cnt = 1
        for i in range(1, len(nums)):
            if nums[i]==nums[i-1]:
                cnt += 1
                if cnt > 2:
                    continue
            else:
                cnt = 1
            
            j += 1
            nums[j] = nums[i]
            
        return j+1
```

上面的代码明显很垃圾。

遍历数组检查每一个元素是否应该被保留，如果应该被保留，就将其移动到指定位置。具体地，我们定义两个指针 slow 和 fast 分别为慢指针和快指针，其中慢指针表示处理出的数组的长度，快指针表示已经检查过的数组的长度。


```python
class Solution(object):
    def removeDuplicates(self, nums):
        if len(nums) <= 2:
            return len(nums)
            
        j = 2
        for i in range(2, len(nums)):
            if nums[i]!=nums[j-2]: # 这边是跟 j-2 的位置比较，用来检查当前的元素是否符合要求。
                nums[j] = nums[i]
                j += 1
            
        return j
```

**通用解法**

为了让解法更具有一般性，我们将原问题的「保留 2 位」修改为「保留 k 位」。

对于此类问题，我们应该进行如下考虑：

* 由于是保留 k 个相同数字，对于前 k 个数字，我们可以直接保留
* 对于后面的任意数字，能够保留的前提是：与当前写入的位置前面的第 k 个元素进行比较，不相同则保留


作者：AC_OIer
链接：https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array-ii/solution/gong-shui-san-xie-guan-yu-shan-chu-you-x-glnq/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


```python
class Solution(object):
    def removeDuplicates(self, nums):
        k = 2

        if len(nums) <= k:
            return len(nums)
        
        j = k
        for i in range(k, len(nums)):
            if nums[i]!=nums[j-k]:
                nums[j] = nums[i]
                j += 1
            
        return j
```

继续优化，去掉边界判断

```python
class Solution(object):
    def removeDuplicates(self, nums):
        k = 2
        j = 0
        for x in nums:
            if j < k or x != nums[j-k]:
                nums[j] = x
                j += 1
        return j
```