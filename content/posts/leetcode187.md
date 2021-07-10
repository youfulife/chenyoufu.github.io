---
title: "187 重复的DNA序列"
date: 2021-07-10T22:15:07+08:00
draft: false
tags: ["算法", "字符串", "滑动窗口", "再刷"]
categories: ["技术"]
---

**题目**

所有 DNA 都由一系列缩写为 'A'，'C'，'G' 和 'T' 的核苷酸组成，例如："ACGAATTCCG"。在研究 DNA 时，识别 DNA 中的重复序列有时会对研究非常有帮助。

编写一个函数来找出所有目标子串，目标子串的长度为 10，且在 DNA 字符串 s 中出现次数超过一次。

示例 1：
```
输入：s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
输出：["AAAAACCCCC","CCCCCAAAAA"]
```
示例 2：
```
输入：s = "AAAAAAAAAAAAA"
输出：["AAAAAAAAAA"]
```

链接：https://leetcode-cn.com/problems/repeated-dna-sequences

**解题思路**

* 和一个字符，判断重复一样，hash存储已经遍历过的。
* 不过这里要去重，所以条件是只要第一次重复的时候才加入到结果中。

```python
class Solution(object):
    def findRepeatedDnaSequences(self, s):
        d = {}
        ans = []
        for i in range(len(s) - 9):
            tmp = s[i:i+10]
            if d.get(tmp, 0) == 1:
                ans.append(tmp)
            d[tmp] = d.get(tmp, 0) + 1
        return ans
```

**长度为L的通用模版**

```python
class Solution:
    def findRepeatedDnaSequences(self, s: str) -> List[str]:
        L, n = 10, len(s)     
        seen, output = set(), set()
        # iterate over all sequences of length L
        for start in range(n - L + 1):
            tmp = s[start: start + L]
            if tmp in seen:
                output.add(tmp[:])
            seen.add(tmp)
        return output
```