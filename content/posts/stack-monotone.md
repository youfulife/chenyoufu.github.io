---
title: "单调栈系列"
date: 2022-04-24T22:47:10+08:00
draft: false
tags: ["算法", "单调栈"]
categories: ["技术"]
---

单调递增栈：单调递增栈就是从栈底到栈顶数据是从大到小
单调递减栈：单调递减栈就是从栈底到栈顶数据是从小到大

则如果栈为空或入栈元素值小于栈顶元素值，则入栈；否则，如果入栈则会破坏栈的单调性，则需要把比入栈元素小的元素全部出栈。单调递减的栈反之。

应用场景：

1.需要找到两边（一边）比cur大（小）的值时可以用，遇到时退栈并计算（更新），栈中一般存储下标（下标和值的pair，或值另外存在vector）

2.需要在无序数列中保证有序数列时，如去掉k位数使得数最小，此时栈中一般保存数而不是下标

模板

```C
for(int i = 0; i < nums.size(); i++){
    while(!st.empty() && nums[i] < st.top()){
        st.pop()
        // 其他计算
    }
     st.push(nums[i]);
}
```

### 题目

* [581. 最短无序连续子数组](https://leetcode.cn/problems/shortest-unsorted-continuous-subarray)
* [901. 股票价格跨度](https://leetcode.cn/problems/online-stock-span/)
* [496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/)
* [84. 柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)
* [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)

### 581. 最短无序连续子数组

给你一个整数数组 nums ，你需要找出一个 连续子数组 ，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

请你找出符合题意的 最短 子数组，并输出它的长度。

```
输入：nums = [2,6,4,8,10,9,15]
输出：5
解释：你只需要对 [6, 4, 8, 10, 9] 进行升序排序，那么整个表都会变为升序排序。
```

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/shortest-unsorted-continuous-subarray
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**思路2**

https://leetcode.cn/problems/shortest-unsorted-continuous-subarray/solution/gong-shui-san-xie-yi-ti-shuang-jie-shuan-e1le/

最终目的是让整个数组有序，那么我们可以先将数组拷贝一份进行排序，然后使用两个指针 i 和 j 分别找到左右两端第一个不同的地方。

```python
class Solution(object):
    def findUnsortedSubarray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        tmp = nums[::]
        tmp.sort()
        left, right = len(nums), 0
        for i in range(len(nums)):
            if nums[i] != tmp[i]:
                left = min(left, i)
                right = max(right, i)
        
        return len(nums[left:right+1])
```

**思路3**

https://leetcode.cn/problems/shortest-unsorted-continuous-subarray/solution/si-lu-qing-xi-ming-liao-kan-bu-dong-bu-cun-zai-de-/

我们可以假设把这个数组分成三段，左段和右段是标准的升序数组，中段数组虽是无序的，但满足最小值大于左段的最大值，最大值小于右段的最小值。

```python
class Solution(object):
    def findUnsortedSubarray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        n = len(nums)
        left = n
        right = 0
        minx = float('+inf')
        maxx = float('-inf')
        for i in range(n):
            if nums[i] < maxx:
                right = i
            else:
                maxx = nums[i]
            
            if nums[n-i-1] > minx:
                left = n-i-1
            else:
                minx = nums[n-i-1]
        return len(nums[left:right+1])
```

用单调栈来查找边界

```python
class Solution(object):
    def findUnsortedSubarray(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        left = len(nums)
        right = 0
        stack = []
        for i in range(len(nums)):
            while stack and nums[stack[-1]] > nums[i]:
                j = stack.pop()
                left = min(left, j)
            stack.append(i)
        stack = []
        for i in range(len(nums)-1, -1, -1):
            while stack and nums[stack[-1]] < nums[i]:
                j = stack.pop()
                right = max(right, j)
            stack.append(i)
        return len(nums[left:right+1])
```


### 901. 股票价格跨度

编写一个 StockSpanner 类，它收集某些股票的每日报价，并返回该股票当日价格的跨度。

今天股票价格的跨度被定义为股票价格小于或等于今天价格的最大连续日数（从今天开始往回数，包括今天）。

例如，如果未来7天股票的价格是 [100, 80, 60, 70, 60, 75, 85]，那么股票跨度将是 [1, 1, 1, 2, 1, 4, 6]。

来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/online-stock-span
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```python
class StockSpanner(object):

    def __init__(self):
        self.stack = []
        self.nums = []
        self.spanner = []

    def next(self, price):
        """
        :type price: int
        :rtype: int
        """
        ans = 1
        i = len(self.nums)
        while self.stack and self.nums[self.stack[-1]] <= price:
            j = self.stack.pop()
            ans = i - j + self.spanner[j]
        self.spanner.append(ans)
        self.stack.append(i)
        self.nums.append(price)
        return ans

# Your StockSpanner object will be instantiated and called as such:
# obj = StockSpanner()
# param_1 = obj.next(price)
```

```python
class StockSpanner(object):

    def __init__(self):
        self.stack = []


    def next(self, price):
        """
        :type price: int
        :rtype: int
        """
        ans = 1
        while self.stack and self.stack[-1][0] <= price:
            weight = self.stack.pop()[1]
            ans += weight
        self.stack.append((price, ans))
        return ans
```


### 496. 下一个更大元素 I

nums1 中数字 x 的 下一个更大元素 是指 x 在 nums2 中对应位置 右侧 的 第一个 比 x 大的元素。

给你两个 没有重复元素 的数组 nums1 和 nums2 ，下标从 0 开始计数，其中nums1 是 nums2 的子集。

对于每个 0 <= i < nums1.length ，找出满足 nums1[i] == nums2[j] 的下标 j ，并且在 nums2 确定 nums2[j] 的 下一个更大元素 。如果不存在下一个更大元素，那么本次查询的答案是 -1 。

返回一个长度为 nums1.length 的数组 ans 作为答案，满足 ans[i] 是如上所述的 下一个更大元素 。



来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/next-greater-element-i
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def nextGreaterElement(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: List[int]
        """
        stack = []
        m = {}
        for i in range(len(nums2)):
            while stack and nums2[stack[-1]] < nums2[i]:
                j = stack.pop()
                m[nums2[j]] = nums2[i]
            stack.append(i)
        ans = []
        for x in nums1:
            ans.append(m.get(x, -1))
        return ans
```

### 402. 移掉 K 位数字

给你一个以字符串表示的非负整数 num 和一个整数 k ，移除这个数中的 k 位数字，使得剩下的数字最小。请你以字符串形式返回这个最小的数字。

 
示例 1 ：
```
输入：num = "1432219", k = 3
输出："1219"
解释：移除掉三个数字 4, 3, 和 2 形成一个新的最小的数字 1219 。
```
来源：力扣（LeetCode）
链接：https://leetcode.cn/problems/remove-k-digits
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def removeKdigits(self, num, k):
        """
        :type num: str
        :type k: int
        :rtype: str
        """
        stack = []
        count = 0
        for i in num:
            while stack and stack[-1] > i and count < k:
                stack.pop()
                count += 1
            stack.append(i)
        ans = ''
        for i in range(len(num) - k):
            if ans == '' and stack[i] == '0':
                continue
            ans += stack[i]
        return ans if ans != '' else '0'
```

### 84. 柱状图中最大的矩形

给定 n 个非负整数，用来表示柱状图中各个柱子的高度。每个柱子彼此相邻，且宽度为 1 。

求在该柱状图中，能够勾勒出来的矩形的最大面积。

输入：heights = [2,1,5,6,2,3]
输出：10
解释：最大的矩形为图中红色区域，面积为 10

**解题思路**

* 固定高度，枚举每个高度的左右边界，取最大值。
* 左边界和右边界都是第一个比当前柱子高度小的，当然不包括左右边界。
* 单调栈正好可以找左右边界，可以加2个哨兵，这样不用处理特殊的边界情况。

```python
class Solution(object):
    def largestRectangleArea(self, heights):
        """
        :type heights: List[int]
        :rtype: int
        """
        nums = heights
        nums = [0] + nums + [0]
        stack = []
        ans = 0
        for i in range(len(nums)):
            while stack and nums[stack[-1]] > nums[i]:
                j = stack.pop()
                height = nums[j]
                right = i - 1
                left = stack[-1] + 1
                width = right - left + 1
                ans = max(ans, height * width)
            stack.append(i)
        return ans
```

### 42. 接雨水

给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。

```
输入：height = [0,1,0,2,1,0,1,3,2,1,2,1]
输出：6
解释：上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 
```
示例 2：
```
输入：height = [4,2,0,3,2,5]
输出：9
```

**解题思路**

```python
class Solution(object):
    def trap(self, height):
        """
        :type height: List[int]
        :rtype: int
        """
        ans = 0
        stack = []
        for i in range(len(height)):
            while stack and height[stack[-1]] < height[i]:
                j = stack.pop()
                h = height[i] - height[j]
                if stack:
                    h = min(h, height[stack[-1]] - height[j])
                    ans += h * (i - stack[-1]-1)
            stack.append(i)
        return ans
```
