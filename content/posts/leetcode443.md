---
title: "443 压缩字符串"
date: 2021-08-21T16:51:09+08:00
draft: false
tags: ["算法", "双指针", "五星"]
categories: ["技术"]
---

**题目**

给你一个字符数组 chars ，请使用下述算法压缩：

从一个空字符串 s 开始。对于 chars 中的每组 连续重复字符 ：

如果这一组长度为 1 ，则将字符追加到 s 中。
否则，需要向 s 追加字符，后跟这一组的长度。
压缩后得到的字符串 s 不应该直接返回 ，需要转储到字符数组 chars 中。需要注意的是，如果组长度为 10 或 10 以上，则在 chars 数组中会被拆分为多个字符。

请在 修改完输入数组后 ，返回该数组的新长度。

你必须设计并实现一个只使用常量额外空间的算法来解决此问题。

示例 1：
```
输入：chars = ["a","a","b","b","c","c","c"]
输出：返回 6 ，输入数组的前 6 个字符应该是：["a","2","b","2","c","3"]
解释：
"aa" 被 "a2" 替代。"bb" 被 "b2" 替代。"ccc" 被 "c3" 替代。
```

链接：https://leetcode-cn.com/problems/string-compression

**解题思路**

空间复杂度O(N), 不符合题目要求。

```python
class Solution(object):

    def compress(self, chars):
        def encode_len(l):
            a = []
            if l == 1:
                return a
            
            while l > 0:
                a.append(str(l % 10))
                l = l / 10
            return a[::-1]
        # 加入一个不可能存在的字符，不用额外处理边界
        chars.append(' ')
        arr = []
        i = 0
        j = 0
        while i < len(chars):
            if chars[i] != chars[j]:
                arr.append(chars[j])
                arr += encode_len(i - j)
                j = i
            i += 1
        
        for i in range(len(arr)):
            chars[i] = arr[i]
        return len(arr)
```

如果是空间复杂度为O(1)，需要原地修改字符数组和写入长度。

```python
class Solution(object):

    def compress(self, chars):
        n = len(chars)
        i = 0 # cur read
        j = 0 # next to write
        left = 0 # cur continue start
        while i < n:
            if i == n - 1 or chars[i] != chars[i+1]:
                cnt = i - left + 1
                chars[j] = chars[i]
                j += 1
                left = i + 1
                if cnt > 1:
                    pos = j
                    while cnt > 0:
                        chars[j] = str(cnt % 10)
                        cnt = cnt / 10
                        j += 1
                    chars[pos:j] = reversed(chars[pos:j])
            i += 1
        return j
```