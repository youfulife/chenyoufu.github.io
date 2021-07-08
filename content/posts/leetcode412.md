---
title: "412 Fizz Buzz"
date: 2021-07-06T11:20:43+08:00
draft: false
tags: ["算法", "字符串", "送分", "简单I"]
categories: ["技术"]
---
**题目**
写一个程序，输出从 1 到 n 数字的字符串表示。

1. 如果 n 是3的倍数，输出“Fizz”；

2. 如果 n 是5的倍数，输出“Buzz”；

3. 如果 n 同时是3和5的倍数，输出 “FizzBuzz”。

链接：https://leetcode-cn.com/problems/fizz-buzz

**解题思路**

```python
class Solution(object):
    def fizzBuzz(self, n):
        ans = []
        for i in range(1, n+1):
            if i % 15 == 0:
                ans.append("FizzBuzz")
            elif i % 3 == 0:
                ans.append("Fizz")
            elif i % 5 == 0:
                ans.append("Buzz")
            else:
                ans.append(str(i))
        return ans
```