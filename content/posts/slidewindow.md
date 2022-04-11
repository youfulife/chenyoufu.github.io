---
title: "滑动窗口"
date: 2022-04-11T21:16:45+08:00
draft: false
tags: ["算法", "滑动窗口", "双指针"]
categories: ["技术"]
---


[643. 子数组最大平均数 I](https://leetcode-cn.com/problems/maximum-average-subarray-i/)
[76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)



##  固定长度的窗口
### 643. 子数组最大平均数 I

给你一个由 n 个元素组成的整数数组 nums 和一个整数 k 。

请你找出平均数最大且 长度为 k 的连续子数组，并输出该最大平均数。

任何误差小于 10-5 的答案都将被视为正确答案。


示例 1：
```
输入：nums = [1,12,-5,-6,50,3], k = 4
输出：12.75
解释：最大平均数 (12-5-6+50)/4 = 51/4 = 12.75
```
来源：力扣（LeetCode）

链接：https://leetcode-cn.com/problems/maximum-average-subarray-i

著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def findMaxAverage(self, nums, k):
        """
        :type nums: List[int]
        :type k: int
        :rtype: float
        """
        s = 0
        for i in range(k):
            s += nums[i]

        ans = s
        
        for i in range(k, len(nums)):
            s = s - nums[i-k] + nums[i]
            ans = max(ans, s)
```


## 不固定长度的窗口

### 76. 最小覆盖子串

给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。

示例 1：
```
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
```
示例 2：
```
输入：s = "a", t = "a"
输出："a"
```

注意：

* 对于 t 中重复字符，我们寻找的子字符串中该字符数量必须不少于 t 中该字符数量。
* 如果 s 中存在这样的子串，我们保证它是唯一的答案。


链接：https://leetcode-cn.com/problems/minimum-window-substring

```python
class Solution(object):
    def minWindow(self, s, t):
        need, window = {}, {}
        for x in t:
            need[x] = need.get(x, 0) + 1
        left, right = 0, 0
        valid = 0
        start, end = 0, 0
        minL = len(s) + 1
        while right < len(s):
            c = s[right]
            right += 1
            if c in need:
                window[c] = window.get(c, 0) + 1
                if window[c] == need[c]:
                    valid += 1
            
            while valid == len(need):
                if right - left < minL:
                    start = left
                    end = right
                    minL = right - left
                
                d = s[left]
                left += 1
                if d in need:
                    if window[d] == need[d]:
                        valid -= 1
                    window[d] -= 1
            
        return s[start: end]
```