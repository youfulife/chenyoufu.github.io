---
title: "239 滑动窗口最大值"
date: 2021-04-10T16:28:40+08:00
draft: false
tags: ["算法", "栈和队列"]
categories: ["技术"]
---

**题目**
给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。

返回滑动窗口中的最大值。

 
**示例 1：**
```
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**示例 2：**

```
输入：nums = [1], k = 1
输出：[1]
```

**提示：**

```
1 <= nums.length <= 105
-104 <= nums[i] <= 104
1 <= k <= nums.length
```

链接：https://leetcode-cn.com/problems/sliding-window-maximum

**解题思路**

**1. 暴力**

```python
class Solution(object):
    def maxSlidingWindow(self, nums, k):
        ans = []
        n = len(nums)
        for i in range(n-k+1):
            ans.append(max(nums[i:i+k]))
        return ans
```

**2. 双端队列 deque**

保留当前窗口内的值从大到小排序，到一个双端队列中。

We scan the array from 0 to n-1, keep "promising" elements in the deque. The algorithm is amortized O(n) as each element is put and polled once.

At each i, we keep "promising" elements, which are potentially max number in window [i-(k-1),i] or any subsequent window. This means

1. If an element in the deque and it is out of i-(k-1), we discard them. We just need to poll from the head, as we are using a deque and elements are ordered as the sequence in the array

2. Now only those elements within [i-(k-1),i] are in the deque. We then discard elements smaller than a[i] from the tail. This is because if a[x] < a[i] and x < i, then a[x] has no chance to be the "max" in [i-(k-1),i], or any other subsequent window: a[i] would always be a better candidate.

3. As a result elements in the deque are ordered in both sequence in array and their value. At each step the head of the deque is the max element in [i-(k-1),i]

```python
class Solution(object):
    def maxSlidingWindow(self, nums, k):
        ans = []
        q = collections.deque()
        for i, num in enumerate(nums):
            while q and nums[q[-1]] < num:
                q.pop()
            while q and q[0] < i-k+1:
                q.popleft()
            q.append(i)
            if i >= k-1: # 这里边界一定要正确
                ans.append(nums[q[0]]) # 做的时候容易忘记 nums, 而只写成q[0]
        return ans
        
```

这类需要用到高级数据结构的题型，要用python或者java这种内置库就提供了相关数据结构的语言，否则自己需要先实现一下数据结构。