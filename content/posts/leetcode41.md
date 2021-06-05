---
title: "41 缺失的第一个正数"
date: 2021-05-29T22:17:11+08:00
draft: false
tags: ["算法", "第一个", "数组"]
categories: ["技术"]
---
**题目**

给你一个未排序的整数数组 nums ，请你找出其中没有出现的最小的正整数。

请你实现时间复杂度为 O(n) 并且只使用常数级别额外空间的解决方案。
 
示例 1：
```
输入：nums = [1,2,0]
输出：3
```
示例 2：
```
输入：nums = [3,4,-1,1]
输出：2
```
示例 3：
```
输入：nums = [7,8,9,11,12]
输出：1
```

链接：https://leetcode-cn.com/problems/first-missing-positive

**解题思路**

**时间复杂度超标**

* 先排序， python的优势就显示出来了。 
* 加一个计数器count，每次遍历的值和正数对比，如果不相同，就返回count的值。
* 一定要注意去重。
* 如果循环完成都相同，返回 count+1。

```python
class Solution(object):
    def firstMissingPositive(self, nums):
        nums.sort()

        count = 0
        for i in range(len(nums)):
            if nums[i] <= 0 :
                continue
            if i > 0 and nums[i] == nums[i-1]: #第一次提交没判断重复
                continue
            count += 1
            if nums[i] != count:
                return count
        return count+1
```

**哈希表方法，同样不满足空间复杂度的条件，但是最简单。**

```python
class Solution(object):
    def firstMissingPositive(self, nums):
        d = {}
        for n in nums:
            d[n] = 1
        
        n = 1
        while True:
            if d.get(n, 0) == 0:
                break
            n += 1
        return n
```

**原地哈希 标志位**

仔细想一想，我们为什么要使用哈希表？这是因为哈希表是一个可以支持快速查找的数据结构：给定一个元素，我们可以在 O(1)的时间查找该元素是否在哈希表中。因此，我们可以考虑将给定的数组设计成哈希表的「替代产品」。

实际上，对于一个长度为 NN 的数组，其中没有出现的最小正整数只能在 [1,N+1] 中。这是因为如果[1,N] 都出现了，那么答案是 N+1，否则答案是 [1,N] 中没有出现的最小正整数。这样一来，我们将所有在 [1, N] 范围内的数放入哈希表，也可以得到最终的答案。而给定的数组恰好长度为 N，这让我们有了一种将数组设计成哈希表的思路：

我们对数组进行遍历，对于遍历到的数 x，如果它在 [1, N] 的范围内，那么就将数组中的第 x-1 个位置（注意：数组下标从 0开始）打上「标记」。在遍历结束之后，如果所有的位置都被打上了标记，那么答案是 N+1，否则答案是最小的没有打上标记的位置加 1。

那么如何设计这个「标记」呢？由于数组中的数没有任何限制，因此这并不是一件容易的事情。但我们可以继续利用上面的提到的性质：由于我们只在意 [1, N] 中的数，因此我们可以先对数组进行遍历，把不在 [1, N]范围内的数修改成任意一个大于 N 的数（例如 N+1）。这样一来，数组中的所有数就都是正数了，因此我们就可以将「标记」表示为「负号」。算法的流程如下：

* 我们将数组中所有小于等于 0 的数修改为 N+1；

* 我们遍历数组中的每一个数 x，它可能已经被打了标记，因此原本对应的数为 |x|。如果 ∣x∣∈[1,N]，那么我们给数组中的第 |x| - 1 个位置的数添加一个负号。注意如果它已经有负号，不需要重复添加；

* 在遍历完成之后，如果数组中的每一个数都是负数，那么答案是 N+1，否则答案是第一个正数的位置加 1。

```python
class Solution(object):
    def firstMissingPositive(self, nums):
        l = len(nums)
        for i, n in enumerate(nums):
            if n <= 0:
                nums[i] = l+1
        
        for i, n in enumerate(nums):
            x = abs(n)
            if x >= 1 and x <= l and nums[x-1] > 0:
                nums[x-1] *= -1
        
        for i, n in enumerate(nums):
            if n > 0:
                return i+1
        return l+1
```

复杂度分析

时间复杂度：O(N)O(N)，其中 NN 是数组的长度。

空间复杂度：O(1)O(1)。


**置换法**

除了打标记以外，我们还可以使用置换的方法，将给定的数组「恢复」成下面的形式：

如果数组中包含 x in [1, N]，那么恢复后，数组的第 x - 1 个元素为 x。

在恢复后，数组应当有 [1, 2, ..., N] 的形式，但其中有若干个位置上的数是错误的，每一个错误的位置就代表了一个缺失的正数。

以题目中的示例二 [3, 4, -1, 1] 为例，恢复后的数组应当为 [1, -1, 3, 4]，我们就可以知道缺失的数为 2。

那么我们如何将数组进行恢复呢？我们可以对数组进行一次遍历，对于遍历到的数 x=nums[i]，如果 x∈[1,N]，我们就知道 x 应当出现在数组中的 x−1 的位置，因此交换 nums[i] 和 nums[x−1]，这样 x 就出现在了正确的位置。在完成交换后，新的 nums[i] 可能还在 [1,N] 的范围内，我们需要继续进行交换操作，直到 x not in [1, N]。

注意到上面的方法可能会陷入死循环。如果 nums[i] 恰好与 nums[x−1] 相等，那么就会无限交换下去。此时我们有 nums[i]=x=nums[x−1]，说明 x 已经出现在了正确的位置。因此我们可以跳出循环，开始遍历下一个数。

由于每次的交换操作都会使得某一个数交换到正确的位置，因此交换的次数最多为 N，整个方法的时间复杂度为 O(N)。

```python
class Solution(object):
    def firstMissingPositive(self, nums):
        n = len(nums)
        for i in range(n):
            while 1 <= nums[i] <= n and nums[nums[i] - 1] != nums[i]: # while 如果写出if就会出错
                nums[nums[i] - 1], nums[i] = nums[i], nums[nums[i] - 1]
        for i in range(n):
            if nums[i] != i + 1:
                return i + 1
        return n + 1
```