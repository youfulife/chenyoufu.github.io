---
title: "467 环绕字符串中唯一的子字符串"
date: 2021-07-25T22:27:37+08:00
draft: false
tags: ["算法", "前缀和", "五星"]
categories: ["技术"]
---
**题目**

把字符串 s 看作是“abcdefghijklmnopqrstuvwxyz”的无限环绕字符串，所以 s 看起来是这样的："...zabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcd....". 

现在我们有了另一个字符串 p 。你需要的是找出 s 中有多少个唯一的 p 的非空子串，尤其是当你的输入是字符串 p ，你需要输出字符串 s 中 p 的不同的非空子串的数目。 

注意: p 仅由小写的英文字母组成，p 的大小可能超过 10000。

示例 1:
```
输入: "a"
输出: 1
解释: 字符串 S 中只有一个"a"子字符。
```

链接：https://leetcode-cn.com/problems/unique-substrings-in-wraparound-string

**解题思路**

```python
class Solution(object):
    def findSubstringInWraproundString(self, p):
        if len(p) == 0:
            return 0
        # 保存最以p[i]结尾的最大子串长度，也就是以p[i]结尾的子数组的个数
        d = {
            p[0]: 1
        }
        s = 1
        for i in range(1, len(p)):
            if ord(p[i]) - ord(p[i-1]) == 1 or ord(p[i]) - ord(p[i-1]) == -25:
                s += 1
            else:
                s = 1
            
            d[p[i]] = max(d.get(p[i], 0), s)
        
        ans = 0
        for k in d:
            ans += d[k]
        return ans
```

参考：

前缀和系列解题思路

* https://leetcode-cn.com/problems/unique-substrings-in-wraparound-string/solution/xi-fa-dai-ni-xue-suan-fa-yi-ci-gao-ding-qian-zhui-/