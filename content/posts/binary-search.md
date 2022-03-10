---
title: "二分查找"
date: 2022-02-28T21:52:53+08:00
draft: false
tags: ["算法", "二分查找"]
categories: ["技术"]
---

### 总结

* 二分查找模板 I

* 初始条件：left = 0, right = length-1
* 终止：left > right
* 向左查找：right = mid-1
* 向右查找：left = mid+1


二分的本质是「二段性」而非「单调性」，而经过本题，我们进一步发现「二段性」还能继续细分，不仅仅只有满足 0101 特性（满足/不满足）的「二段性」可以使用二分，满足 1?1? 特性（一定满足/不一定满足）也可以二分。

作者：AC_OIer
链接：https://leetcode-cn.com/problems/find-peak-element/solution/gong-shui-san-xie-noxiang-xin-ke-xue-xi-qva7v/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。


### 经典题目

* [704. 二分查找](https://leetcode-cn.com/problems/binary-search/)
* [69. x 的平方根](https://leetcode-cn.com/problems/sqrtx/)
* [33. 搜索旋转排序数组](https://leetcode-cn.com/problems/search-in-rotated-sorted-array/)
* [278. 第一个错误的版本](https://leetcode-cn.com/problems/first-bad-version/)
* [153. 寻找旋转排序数组中的最小值](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array/)
* [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/)
* [162. 寻找峰值](https://leetcode-cn.com/problems/find-peak-element/)
* [658. 找到 K 个最接近的元素](https://leetcode-cn.com/problems/find-k-closest-elements/)


### 704. 二分查找

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。

**解题思路**

定义查找的范围[left,right] 闭区间。

四个月过去，二分几乎忘光了,错误就有3处。

1. 边界条件 right = len(nums)-1 忘记-1了，导致出现死循环。
2. left = mid + 1 直接写成left = mid
3. right = mid - 1 直接写成right = mid

```python
class Solution(object):
    def search(self, nums, target):
        left = 0
        
        right = len(nums)-1
       
        while left <= right:
            mid = left + (right-left)/2
            if nums[mid] < target:
                left = mid + 1
            elif nums[mid] > target:
                right = mid - 1
            else:
                return mid
        return -1
```

### 69. x 的平方根

给你一个非负整数 x ，计算并返回 x 的 算术平方根 。

由于返回类型是整数，结果只保留 整数部分 ，小数部分将被 舍去 。

注意：不允许使用任何内置指数函数和算符，例如 pow(x, 0.5) 或者 x ** 0.5 。

```python
class Solution(object):
    def mySqrt(self, x):
        left = 0
        right = x
        ans = -1
        while left<=right:
            mid = left + (right-left)/2
            if mid * mid <= x:
                ans = mid
                left = mid+1
            else:
                right = mid-1
        return ans
```

### 33. 搜索旋转排序数组

整数数组 nums 按升序排列，数组中的值 互不相同 。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为 [4,5,6,7,0,1,2] 。

给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回 -1 。


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/search-in-rotated-sorted-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def search(self, nums, target):
        left = 0
        right = len(nums)-1
        while left <= right:
            mid = left + (right-left)/2
            x = nums[mid]
            if x == target:
                return mid
            # 不能忘记 = nums[0] 的情况
            if x >= nums[0]:
                # 不能忘记 >= nums[left] 的情况
                if nums[left]<=target < x:
                    right = mid - 1
                else:
                    left = mid + 1
            else:
                # 不能忘记 <>= nums[right] 的情况
                if x < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
        return -1
```

### 278. 第一个错误的版本

你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 n 个版本 [1, 2, ..., n]，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用 bool isBadVersion(version) 接口来判断版本号 version 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/first-bad-version
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return a bool
# def isBadVersion(version):

class Solution(object):
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        left = 1
        right = n
        ans = 1
        while left <= right:
            mid = left + (right-left)/2
            if isBadVersion(mid):
                ans = mid
                right = mid-1
            else:
                left = mid + 1
        return ans
```

### 153. 寻找旋转排序数组中的最小值

已知一个长度为 n 的数组，预先按照升序排列，经由 1 到 n 次 旋转 后，得到输入数组。例如，原数组 nums = [0,1,2,4,5,6,7] 在变化后可能得到：
若旋转 4 次，则可以得到 [4,5,6,7,0,1,2]
若旋转 7 次，则可以得到 [0,1,2,4,5,6,7]
注意，数组 [a[0], a[1], a[2], ..., a[n-1]] 旋转一次 的结果为数组 [a[n-1], a[0], a[1], a[2], ..., a[n-2]] 。

给你一个元素值 互不相同 的数组 nums ，它原来是一个升序排列的数组，并按上述情形进行了多次旋转。请你找出并返回数组中的 最小元素 。

你必须设计一个时间复杂度为 O(log n) 的算法解决此问题。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def findMin(self, nums):
        left = 0
        right = len(nums) - 1
        while left<right:
            mid = left + (right-left)/2
            if nums[mid] > nums[right]:
                left = mid + 1
            else:
                right = mid
        return nums[left]
```

### 34. 在排序数组中查找元素的第一个和最后一个位置

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回 [-1, -1]。

进阶：

你可以设计并实现时间复杂度为 O(log n) 的算法解决此问题吗？


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**解题思路**


```python
class Solution(object):
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        def leftBound(nums, target):
            left = 0
            right = len(nums) - 1
            ans = -1
            while left <= right:
                mid = left + (right-left)/2
                if nums[mid] < target:
                    left = mid + 1
                elif nums[mid] > target:
                    right = mid - 1
                else:
                    ans = mid
                    # leftBound和rightBound的唯一的不同点就是这一句，其余的都一摸一样。
                    right = mid - 1
            return ans
        
        def rightBound(nums, target):
            left = 0
            right = len(nums) - 1
            ans = -1
            while left <= right:
                mid = left + (right-left)/2
                if nums[mid] < target:
                    left = mid + 1
                elif nums[mid] > target:
                    right = mid - 1
                else:
                    ans = mid
                    # 
                    left = mid + 1
            return ans
        return [leftBound(nums, target), rightBound(nums, target)]
```

### 162. 寻找峰值

峰值元素是指其值严格大于左右相邻值的元素。

给你一个整数数组 nums，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回 任何一个峰值 所在位置即可。

你可以假设 nums[-1] = nums[n] = -∞ 。

你必须实现时间复杂度为 O(log n) 的算法来解决此问题。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/find-peak-element
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def findPeakElement(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        left = 0
        right = len(nums)-1
        while left < right:
            mid = left + (right-left) / 2
            if nums[mid] > nums[mid+1]:
                right = mid
            else:
                left = mid + 1
        return left
```

### 658. 找到 K 个最接近的元素

给定一个 排序好 的数组 arr ，两个整数 k 和 x ，从数组中找到最靠近 x（两数之差最小）的 k 个数。返回的结果必须要是按升序排好的。

整数 a 比整数 b 更接近 x 需要满足：

|a - x| < |b - x| 或者
|a - x| == |b - x| 且 a < b
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/find-k-closest-elements
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**解题思路**

双指针对撞

```python
class Solution(object):
    def findClosestElements(self, arr, k, x):
        left = 0
        right = len(arr) - 1
        while right - left + 1 > k:
            if x - arr[left] <= arr[right] - x:
                right -= 1
            else:
                left += 1
        return arr[left: left + k]
```

二分, 寻找左边界

```python
class Solution(object):
    def findClosestElements(self, arr, k, x):
        """
        :type arr: List[int]
        :type k: int
        :type x: int
        :rtype: List[int]
        """
        left = 0
        right = len(arr) - k
        while left < right:
            mid = left + (right-left)/2
            if x - arr[mid] <= arr[mid + k] - x:
                right = mid
            else:
                left = mid + 1
        return arr[left: left + k]
```