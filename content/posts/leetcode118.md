---
title: "118 杨辉三角"
date: 2021-07-04T09:40:01+08:00
draft: false
tags: ["算法", "数组"]
categories: ["技术"]
---

**题目**

给定一个非负整数 numRows，生成杨辉三角的前 numRows 行。

在杨辉三角中，每个数是它左上方和右上方的数的和。

示例:
```
输入: 5
输出:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

链接：https://leetcode-cn.com/problems/pascals-triangle

**解题思路**

```python
class Solution(object):
    def generate(self, numRows):
        ans = []
        for i in range(numRows):
            row = [1]
            for j in range(1, i+1):
                if j == i:
                    row.append(1)
                else:
                    row.append(ans[i-1][j-1]+ ans[i-1][j])
            ans.append(row)
        return ans
```