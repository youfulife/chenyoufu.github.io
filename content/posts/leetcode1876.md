---
title: "1876 长度为三且各字符不同的子字符串"
date: 2021-07-10T21:46:57+08:00
draft: false
tags: ["算法", "字符串", "滑动窗口", "简单"]
categories: ["技术"]
---

**题目**

如果一个字符串不含有任何重复字符，我们称这个字符串为 好 字符串。

给你一个字符串 s ，请你返回 s 中长度为 3 的 好子字符串 的数量。

注意，如果相同的好子字符串出现多次，每一次都应该被记入答案之中。

子字符串 是一个字符串中连续的字符序列。

示例 1：
```
输入：s = "xyzzaz"
输出：1
解释：总共有 4 个长度为 3 的子字符串："xyz"，"yzz"，"zza" 和 "zaz" 。
唯一的长度为 3 的好子字符串是 "xyz" 。
```

链接：https://leetcode-cn.com/problems/substrings-of-size-three-with-distinct-characters

**解题斯理**

**遍历**

针对长度为3的还好，如果长度增大到4以上，直接if语句挨个对比就行不通了。

```python
class Solution(object):
    def countGoodSubstrings(self, s):
        ans = 0
        for i in range(len(s)-2):
            if s[i] != s[i+1] and s[i] != s[i+2] and s[i+1] != s[i+2]:
                ans += 1
        return ans
```

**滑动窗口**

```python
class Solution(object):
    def countGoodSubstrings(self, s):
        window = {}
        # [left, right)
        left = 0
        right = 0 
        ans = 0
        while right < len(s):
            c = s[right]
            window[c] = window.get(c, 0) + 1
            right += 1
            # 边界一定要准确
            while window[c] > 1 or right - left > 3:
                window[s[left]] -= 1
                left += 1
            
            if right - left == 3:
                ans += 1
        return ans
```