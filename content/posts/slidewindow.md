---
title: "滑动窗口"
date: 2022-04-11T21:16:45+08:00
draft: false
tags: ["算法", "滑动窗口", "双指针"]
categories: ["技术"]
---


* [643. 子数组最大平均数 I](https://leetcode-cn.com/problems/maximum-average-subarray-i/)
* [76. 最小覆盖子串](https://leetcode-cn.com/problems/minimum-window-substring/)
* [424. 替换后的最长重复字符](https://leetcode-cn.com/problems/longest-repeating-character-replacement/)
* [1423. 可获得的最大点数](https://leetcode-cn.com/problems/maximum-points-you-can-obtain-from-cards)
* [1658. 将 x 减到 0 的最小操作数](https://leetcode-cn.com/problems/minimum-operations-to-reduce-x-to-zero)
* [567. 字符串的排列](https://leetcode-cn.com/problems/permutation-in-string/)

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

### 1423. 可获得的最大点数

几张卡牌 排成一行，每张卡牌都有一个对应的点数。点数由整数数组 cardPoints 给出。

每次行动，你可以从行的开头或者末尾拿一张卡牌，最终你必须正好拿 k 张卡牌。

你的点数就是你拿到手中的所有卡牌的点数之和。

给你一个整数数组 cardPoints 和整数 k，请你返回可以获得的最大点数。

示例 1：
```
输入：cardPoints = [1,2,3,4,5,6,1], k = 3
输出：12
解释：第一次行动，不管拿哪张牌，你的点数总是 1 。但是，先拿最右边的卡牌将会最大化你的可获得点数。最优策略是拿右边的三张牌，最终点数为 1 + 6 + 5 = 12 。
```

**解题思路**

```python
class Solution(object):
    def maxScore(self, cardPoints, k):
        """
        :type cardPoints: List[int]
        :type k: int
        :rtype: int
        """
        n = len(cardPoints)
        s = 0
        for i in range(n-k):
            s += cardPoints[i]
        
        least = s
        for i in range(n-k, n):
            s = s + cardPoints[i] - cardPoints[i-(n-k)]
            least = min(least, s)
        
        return sum(cardPoints) - least
```

**换一种思维，将数组看成是环形数组**

求 [n-k, n) .. [0, k) 这个范围内的k个数的最大的连续子序列和。

```python
class Solution(object):
    def maxScore(self, cardPoints, k):
        n = len(cardPoints)
        s = 0
        maxs = 0
        for i in range(n-k, n):
            s += cardPoints[i]
        maxs = s
        for i in range(0, k):
            s = s + cardPoints[i] - cardPoints[n-k+i]
            maxs = max(maxs, s)
        return maxs
```

### 567 字符串的排列


**题目**

给定两个字符串 s1 和 s2，写一个函数来判断 s2 是否包含 s1 的排列。

换句话说，第一个字符串的排列之一是第二个字符串的 子串 。

示例 1：
```
输入: s1 = "ab" s2 = "eidbaooo"
输出: True
解释: s2 包含 s1 的排列之一 ("ba").
```

**解题思路**

```python
class Solution(object):
    def checkInclusion(self, s1, s2):
        need = {}
        for x in s1:
            need[x] = need.get(x, 0) + 1

        left, right = 0, 0
        window = {}
        valid = 0
        while right < len(s2):
            c = s2[right]
            right += 1
            window[c] = window.get(c, 0) + 1
            if c in need and window[c] == need[c]:
                valid += 1

            while right - left > len(s1):
                d = s2[left]
                left += 1
                if d in need and window[d] == need[d]:
                    valid -= 1
                window[d] -= 1
                if window[d] == 0:
                    del window[d]

            if valid == len(need) and right-left == len(s1):
                return True
        return False
```

**继续优化代码**

* 跟 438 找到字符串中所有字母异位词 https://youfu.life/posts/leetcode438/ 其实是同一个。
* 只要保证窗口内都是满足条件的字符就可以。

```python
class Solution(object):
    def checkInclusion(self, s1, s2):
        need = {}
        for x in s1:
            need[x] = need.get(x, 0) + 1
        left, right = 0, 0
        window = {}
        while right < len(s2):
            c = s2[right]
            right += 1
            window[c] = window.get(c, 0) + 1

            while window[c] > need.get(c, 0):
                d = s2[left]
                left += 1
                window[d] -= 1

            if right-left == len(s1):
                return True
        return False
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

### 424. 替换后的最长重复字符

**题目**

给你一个仅由大写英文字母组成的字符串，你可以将任意位置上的字符替换成另外的字符，总共可最多替换 k 次。在执行上述操作后，找到包含重复字母的最长子串的长度。

注意：字符串长度 和 k 不会超过 104。

示例 1：
```
输入：s = "ABAB", k = 2
输出：4
解释：用两个'A'替换为两个'B',反之亦然。
```

链接：https://leetcode-cn.com/problems/longest-repeating-character-replacement

**解题思路**

* 滑动窗口，枚举字符串中的每一个位置作为右端点，然后找到其最远的左端点的位置，满足该区间内除了出现次数最多的那一类字符之外，剩余的字符（即非最长重复字符）数量不超过 k 个。

```python
class Solution(object):
    def characterReplacement(self, s, k):
        left = 0
        right = 0
        ans = 0
        window = {}
        maxV = 0
        while right < len(s):
            c = s[right]
            right += 1
            window[c] = window.get(c, 0) + 1

            maxV = max(window.values())
    
            if right - left > maxV + k:
                d = s[left]
                left += 1
                window[d] -= 1
            ans = max(ans, right - left)
        return ans
```

贪心+滑动窗口

参考：https://leetcode-cn.com/problems/longest-repeating-character-replacement/solution/ti-huan-hou-de-zui-chang-zhong-fu-zi-fu-eaacp/

right - left在整个过程是非递减的。只要right 的值加进去不满足条件，left和right就一起右滑，因为长度小于right - left的区间就没必要考虑了，所以right - left一直保持为当前的最大值.

维护历史最大值的原因：窗口从根本来说是只增不减的：

如果k够用来替换的话，right右移1步，窗口变大（当前维护的最大值变大，不消耗k；当前维护的最大值不变，将消耗k）；

如果k不够用来替换的话，left和right都右移一步，窗口不变（我们已经不关心比当前小的情况了）。

从这个角度来说，也不难理解最后答案是right-left（最后窗口的大小，可扩充的最大窗口）。

自己最开始在纠结于左指针 left++ 但不更新maxn会不会影响答案，琢磨了好一会才想到：当没有更大的maxn出现时，是不会产生更大的最终结果的，细还是官方题解细啊。

```python
class Solution(object):
    def characterReplacement(self, s, k):
        m = {}
        left = 0
        right = 0
        most = 0
        
        while right < len(s):
            c = s[right]
            right += 1

            m[c] = m.get(c, 0) + 1
            most = max(most, m[c])
            if most + k < right - left:
                d = s[left]
                left += 1
                m[d] -= 1
        return right-left
```

### 1658. 将 x 减到 0 的最小操作数

**题目**
给你一个整数数组 nums 和一个整数 x 。每一次操作时，你应当移除数组 nums 最左边或最右边的元素，然后从 x 中减去该元素的值。请注意，需要 修改 数组以供接下来的操作使用。

如果可以将 x 恰好 减到 0 ，返回 最小操作数 ；否则，返回 -1 。

提示：

* 1 <= nums.length <= 105
* 1 <= nums[i] <= 104
* 1 <= x <= 109

链接：https://leetcode-cn.com/problems/minimum-operations-to-reduce-x-to-zero

**解题思路** 

和最大点数的那个题目类似，可以换个角度来做题。

* 最短的连续子序列和为x，要从头或者尾巴开始
* 转化为求和为 sum(nums)-x 的最长连续子序列
* 用数组长度减去最长连续子序列，剩下的就是和为x的最短连续子序列

```python
class Solution(object):
    def minOperations(self, nums, x):
        s = sum(nums) - x
        left = 0
        right = 0
        # 不能设置为0, 如果sum(nums) == x就会通不过，除非提前判断这种情况。
        # if s == 0:
        #    return len(nums)
        maxl = -1
        cur = 0
        while right < len(nums):
            c = nums[right]
            right += 1
            cur += c

            while left<right and cur > s:
                d = nums[left]
                left += 1
                cur -= d
            
            if cur == s:
                maxl = max(maxl, right-left)
        return -1 if maxl == -1 else len(nums) - maxl
```
