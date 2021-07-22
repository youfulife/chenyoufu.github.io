---
title: "643 子数组最大平均数 I"
date: 2021-07-10T11:48:48+08:00
draft: false
tags: ["算法", "滑动窗口"]
categories: ["技术"]
---
**题目**

给定 n 个整数，找出平均数最大且长度为 k 的连续子数组，并输出该最大平均数。

示例：
```
输入：[1,12,-5,-6,50,3], k = 4
输出：12.75
解释：最大平均数 (12-5-6+50)/4 = 51/4 = 12.75
```

链接：https://leetcode-cn.com/problems/maximum-average-subarray-i

**解题思路**

拿到题目自己的思考

1. 连续，长度K，滑动窗口？

```python
class Solution(object):
    def findMaxAverage(self, nums, k):
        window = deque([])
        right = 0
        ans = float('-inf')
        while right < len(nums):
            c = nums[right]
            window.append(c)
            right += 1

            while len(window) > k:
                window.popleft()
                
            if len(window) == k:
                ans = max(ans, sum(window) * 1.0 / k)
        return ans
```

上面代码超时。

```python
class Solution(object):
    def findMaxAverage(self, nums, k):
        #dp[i] = dp[i-1] + nums[i] - nums[i-k]
        ans = float('-inf')
        sum = 0
        for i in range(len(nums)):
            sum += nums[i]
            if i >= k-1:
                if i > k-1:
                    sum -= nums[i-k]
                ans = max(ans, sum * 1.0 / k)
        return ans
```

优化代码逻辑，更清晰

```python
class Solution(object):
    def findMaxAverage(self, nums, k):
        #dp[i] = dp[i-1] + nums[i] - nums[i-k]
        maxsum = 0
        sum = 0
        for i in range(k):
            sum += nums[i]
        maxsum = sum
        for i in range(k, len(nums)):
            sum = sum + nums[i] - nums[i-k]
            maxsum = max(maxsum, sum)
            
        return maxsum * 1.0 / k
```