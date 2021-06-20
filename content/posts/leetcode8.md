---
title: "8 字符串转换整数 (atoi)"
date: 2021-06-20T13:45:45+08:00
draft: false
tags: ["算法", "字符串"]
categories: ["技术"]
---

**题目**

请你来实现一个 myAtoi(string s) 函数，使其能将字符串转换成一个 32 位有符号整数（类似 C/C++ 中的 atoi 函数）。

函数 myAtoi(string s) 的算法如下：

* 读入字符串并丢弃无用的前导空格
* 检查下一个字符（假设还未到字符末尾）为正还是负号，读取该字符（如果有）。 确定最终结果是负数还是正数。 如果两者都不存在，则假定结果为正。
* 读入下一个字符，直到到达下一个非数字字符或到达输入的结尾。字符串的其余部分将被忽略。
* 将前面步骤读入的这些数字转换为整数（即，"123" -> 123， "0032" -> 32）。如果没有读入数字，则整数为 0 。必要时更改符号（从步骤 2 开始）。
* 如果整数数超过 32 位有符号整数范围 [−231,  231 − 1] ，需要截断这个整数，使其保持在这个范围内。具体来说，小于 −231 的整数应该被固定为 −231 ，大于 231 − 1 的整数应该被固定为 231 − 1 。
* 返回整数作为最终结果。
注意：

* 本题中的空白字符只包括空格字符 ' ' 。
* 除前导空格或数字后的其余字符串外，请勿忽略 任何其他字符。

```
输入：s = "   -42"
输出：-42

输入：s = "4193 with words"
输出：4193

输入：s = "words and 987"
输出：0
```

链接：https://leetcode-cn.com/problems/string-to-integer-atoi


**解题思路**

```python
INT_MAX = 2 ** 31 - 1
INT_MIN = -2 ** 31

class Solution(object):
    def isOverflow(self, v, c, sign):
        if sign == 1:
            if v > INT_MAX / 10 or (v == INT_MAX / 10 and c > INT_MAX % 10):
                return True
        
        if sign == -1:
            if v > -1 * INT_MIN / 10 or (v == -1 * INT_MIN / 10 and c > -1 * INT_MIN % 10):
                return True
        return False

    def myAtoi(self, s):
        token = 0
        pos = 0
        sign = 1
        # 去掉前导空格
        while pos < len(s):
            if s[pos] == ' ':
                pos += 1
                continue
            else:
                break
        # 检查 + - 号
        if pos < len(s) and s[pos] in ['-', '+']:
            if s[pos] == '-':
                sign = -1
            pos += 1
        # 数字
        while pos < len(s):
            cur = s[pos]
            if cur.isdigit():
                # 提前判断是否溢出
                if self.isOverflow(token, int(cur), sign):
                    return INT_MAX if sign == 1 else INT_MIN
                token = token * 10 + int(cur)
            else:
                break
            pos += 1

        return token * sign
```