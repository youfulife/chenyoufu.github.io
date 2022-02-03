---
title: "N数之和问题"
date: 2022-01-29T10:03:20+08:00
draft: false
tags: ["算法", "n数之和", "双指针"]
categories: ["技术"]
---

**题目**

**两数之和**

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

提示：

只会存在一个有效答案

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/two-sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        m = {}
        for i in range(len(nums)):
            x = nums[i]
            y = target - nums[i]

            if y in m:
                return [m[y], i]
            
            m[x] = i
```

**三数之和**

给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

```python
class Solution(object):
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        n = len(nums)
        nums.sort()
        ans = []
        # print(nums)
        for i in range(n):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            
            left = i+1
            right = n - 1
            while left < right:
                # print(left, right)
                if left > i+1  and nums[left] == nums[left-1]:
                    left += 1
                    continue
                if right < n-1 and nums[right] == nums[right+1]:
                    right -= 1
                    continue
                if nums[i] + nums[left] + nums[right] == 0:
                    ans.append([nums[i], nums[left], nums[right]])
                    left += 1
                    right -= 1
                elif nums[i] + nums[left] + nums[right] < 0:
                    left += 1
                else:
                    right -= 1
        return ans
```

```python
class Solution(object):
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        n = len(nums)
        nums.sort()
        ans = []
        target = 0
        for i in range(n):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            j = i+1
            k = n-1
            while j < k:
                s = nums[i] + nums[j] + nums[k]
                if s < target:
                    j += 1
                elif s > target:
                    k -= 1
                else:
                    ans.append([nums[i], nums[j], nums[k]])
                    
                    while j < k and nums[j] == nums[j+1]:
                        j += 1
                    while j < k and nums[k] == nums[k-1]:
                        k -= 1
                    j += 1
                    k -= 1
        return ans
```

**4个数之和**

给你一个由 n 个整数组成的数组 nums ，和一个目标值 target 。请你找出并返回满足下述全部条件且不重复的四元组 [nums[a], nums[b], nums[c], nums[d]] （若两个四元组元素一一对应，则认为两个四元组重复）：

0 <= a, b, c, d < n
a、b、c 和 d 互不相同
nums[a] + nums[b] + nums[c] + nums[d] == target
你可以按 任意顺序 返回答案 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/4sum
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def fourSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        nums.sort()

        def nSum(arr, n, t, path, ans):
            # 注意边界
            if len(arr) < n or n < 2:
                return

            if n == 2:
                # 这里需要去重，而且不止一组答案，所以不用hash方法。
                l = 0
                r = len(arr) - 1
                
                while l < r:
                    if arr[l] + arr[r] == t:
                        ans.append(path + [arr[l], arr[r]])
                        l+=1
                        r-=1
                        while l < r and arr[l] == arr[l-1]:
                            l+=1
                        while l < r and arr[r] == arr[r+1]:
                            r-=1
                    elif arr[l] + arr[r] < t:
                        l+=1
                    else:
                        r-=1
                return
            
            # 注意range是 len(arr)，而不是n
            for i in range(len(arr)):
                if i > 0 and arr[i-1] == arr[i]:
                    continue
                nSum(arr[i+1:], n-1, t - arr[i], path+[arr[i]], ans)
            return
        
        ans = []
        nSum(nums, 4, target, [], ans)
        return ans
```

**最接近的三数之和**

给你一个长度为 n 的整数数组 nums 和 一个目标值 target。请你从 nums 中选出三个整数，使它们的和与 target 最接近。

返回这三个数的和。

假定每组输入只存在恰好一个解。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/3sum-closest
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        nums.sort()
        ans = float('+inf')
        for i in range(len(nums)):
            l = i+1
            r = len(nums)-1
            while l<r:
                s = nums[i] + nums[l] + nums[r]
                if abs(s - target) < abs(ans - target):
                    ans = s
                if  s > target:
                    r -= 1
                elif s < target:
                    l += 1
                else:
                    return s
        return ans
```

**四数相加**

给你四个整数数组 nums1、nums2、nums3 和 nums4 ，数组长度都是 n ，请你计算有多少个元组 (i, j, k, l) 能满足：

* 0 <= i, j, k, l < n
* nums1[i] + nums2[j] + nums3[k] + nums4[l] == 0
 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/4sum-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def fourSumCount(self, nums1, nums2, nums3, nums4):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :type nums3: List[int]
        :type nums4: List[int]
        :rtype: int
        """
        s1, s2 = [], []
        m = {}
        ans = 0
        for x in nums1:
            for y in nums2:
                s1.append(x + y)
                m[x+y] = m.get(x+y, 0) + 1
        for x in nums3:
            for y in nums4:
                if -(x+y) in m:
                    ans += m[-x-y]            
        return ans
```

**923. 三数之和的多种可能**

给定一个整数数组 arr ，以及一个整数 target 作为目标值，返回满足 i < j < k 且 arr[i] + arr[j] + arr[k] == target 的元组 i, j, k 的数量。

由于结果会非常大，请返回 109 + 7 的模。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/3sum-with-multiplicity
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def threeSumMulti(self, arr, target):
        """
        :type arr: List[int]
        :type target: int
        :rtype: int
        """

        arr.sort()
        n = len(arr)
        res = []
        for i in range(n):
            if i> 0 and arr[i] == arr[i-1]:
                continue
            left = i+1
            right = n-1
            while left<right:

                if left > i+1 and arr[left] == arr[left-1]:
                    left += 1
                    continue
                if right<n-1 and arr[right] == arr[right+1]:
                    right -= 1
                    continue

                if arr[i] + arr[left] + arr[right] == target:
                    res.append([arr[i], arr[left], arr[right]])
                    left += 1
                    right -= 1
                elif arr[i] + arr[left] + arr[right] < target:
                    left += 1
                else:
                    right -= 1
        ans = 0
        m = {}
        for x in arr:
            m[x] = m.get(x, 0) + 1
        for x, y, z in res:
            if x == y and y != z:
                ans += m[x] * (m[x] - 1) / 2 * m[z]
            elif x != y and y == z:
                ans += m[y] * (m[y] - 1) / 2 * m[x]
            elif x==y and y==z:
                ans += m[x] * (m[x] - 1) * (m[x]-2)/ 6
            else:
                ans += m[x] * m[y] * m[z]

        return ans % (10 ** 9 + 7)
```

```python
class Solution(object):
    def threeSumMulti(self, arr, target):
        """
        :type arr: List[int]
        :type target: int
        :rtype: int
        """

        arr.sort()
        n = len(arr)
        ans = 0

        for i in range(n):
            left = i+1
            right = n-1
            while left<right:
                if arr[i] + arr[left] + arr[right] == target:
                    if arr[left] == arr[right]:
                        ans += (right-left + 1) * (right-left)/2
                        break                    
                    x, y = 1, 1
                    while left<right and arr[left] == arr[left+1]:
                        x += 1
                        left += 1
                    while left<right and arr[right] == arr[right-1]:
                        y += 1
                        right -= 1
                    ans += x * y
                    left += 1
                    right -= 1
      
                elif arr[i] + arr[left] + arr[right] < target:
                    left += 1
                else:
                    right -= 1


        return ans % (10 ** 9 + 7)
```

**259. 较小的三数之和**

给定一个长度为 n 的整数数组和一个目标值 target，寻找能够使条件 nums[i] + nums[j] + nums[k] < target 成立的三元组  i, j, k 个数（0 <= i < j < k < n）。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/3sum-smaller
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

不需要去重复。

```python
class Solution(object):
    def threeSumSmaller(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        n = len(nums)
        nums.sort()
        ans = 0
        for i in range(n):
            # if i > 0 and nums[i] == nums[i-1]:
            #     continue
            
            j = i + 1
            k = n - 1
            while j<k:
                # if j > i+1 and nums[j] == nums[j-1]:
                #     j += 1
                #     continue
                # if k < n-1 and nums[k] == nums[k+1]:
                #     k -= 1
                #     continue
                s = nums[i] + nums[j] + nums[k]
                if s<target:
                    ans += (k-j)
                    # kk = k
                    # while j<kk:
                    #     ans.append([nums[i], nums[j], nums[kk]])
                    #     kk -= 1
                    j += 1
                else:
                    k -= 1
        # print(ans)
        return ans
```