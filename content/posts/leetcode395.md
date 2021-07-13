---
title: "395 至少有 K 个重复字符的最长子串"
date: 2021-07-13T20:09:23+08:00
draft: false
tags: ["算法", "字符串", "滑动窗口", "递归", "分治", "再刷"]
categories: ["技术"]
---

**题目**

给你一个字符串 s 和一个整数 k ，请你找出 s 中的最长子串， 要求该子串中的每一字符出现次数都不少于 k 。返回这一子串的长度。

示例 1：
```
输入：s = "aaabb", k = 3
输出：3
解释：最长子串为 "aaa" ，其中 'a' 重复了 3 次。
```
提示：

* 1 <= s.length <= 104
* s 仅由小写英文字母组成
* 1 <= k <= 105

链接：https://leetcode-cn.com/problems/longest-substring-with-at-least-k-repeating-characters

**解题思路**

* 一开始看到子串，最少K，这几个字样首先想到滑动窗口，但是想了好长时间没有想出来应该怎么控制窗口的移动
* 看了题解才明白，可以基于提示中s 仅由小写英文字母组成，来控制字符的种类
* 通过穷举26种可能性，来计算最大长度
* 「当确定了窗口内所包含的字符数量时，区间重新具有了二段性质」。这是本题的滑动窗口解法和迄今为止做的滑动窗口题目的最大不同，本题需要手动增加限制，即限制窗口内字符种类。


```python
# 自己写的第一遍，很粗糙，不过调试了几次好在过了
class Solution(object):
    def longestSubstring(self, s, k):
        ans = 0
        for i in range(1, 27):

            left = 0
            right = 0
            window = {}
            valid = 0
            types = 0
            while right < len(s):
                c = s[right]
                right += 1

                window[c] = window.get(c, 0) + 1
                if window[c] == k: # 更新满足条件的字符数
                    valid += 1
                if window[c] == 1: # 更新字符种类
                    types += 1
                # 字符种类大于限制的时候，更新窗口
                while types > i:
                    window[s[left]] -= 1
                    if window[s[left]] == 0:
                        types -= 1
                    if window[s[left]] == k - 1: # 这里要注意，一开始写成了 < k 导致多减了。
                        valid -= 1
                    left += 1
                if valid == i: # 所有字符都满足最少重复k次，才更新最大长度
                    ans = max(ans, right-left)
        return ans
```

**分治+递归**

* 思路，s中所有出现的少于k的字符都不满足要求，所以可以以这个为分割点，将s分割成多个子串，求所有子串的最大长度。

```python
class Solution(object):
    def longestSubstring(self, s, k):
        if len(s) < k:
            return 0
        for c in set(s):
            if s.count(c) < k:
                return max([self.longestSubstring(t, k) for t in s.split(c)])
        return len(s)
```


**参考**

* https://leetcode-cn.com/problems/longest-substring-with-at-least-k-repeating-characters/solution/xiang-jie-mei-ju-shuang-zhi-zhen-jie-fa-50ri1/

解决思路：当我们采用常规的分析思路发现无法进行时，要去关注一下数据范围中「数值小」的值。因为数值小其实是代表了「可枚举」，往往是解题或者降低复杂度的一个重要（甚至是唯一）的突破口。

* https://leetcode-cn.com/problems/longest-substring-with-at-least-k-repeating-characters/solution/jie-ben-ti-bang-zhu-da-jia-li-jie-di-gui-obla/

递归函数到底怎么一层层展开与终止的，不要用大脑去想，这是计算机干的事。我们只用把递归函数当做一个能解决问题的黑箱就够了，把更多的注意力放在拆解子问题、递归终止条件、递归函数的正确性上来。
