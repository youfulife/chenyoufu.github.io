---
title: "9 回文数"
date: 2021-08-21T23:07:23+08:00
draft: false
tags: ["算法", "回文", "五星"]
categories: ["技术"]
---

**题目**

给你一个整数 x ，如果 x 是一个回文整数，返回 true ；否则，返回 false 。

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。例如，121 是回文，而 123 不是。

你能不将整数转为字符串来解决这个问题吗？

链接：https://leetcode-cn.com/problems/palindrome-number

**解题思路**

转成字符串

```python
class Solution(object):
    def isPalindrome(self, x):
        s = str(x)
        i = 0
        j = len(s) - 1
        while i < j:
            if s[i] != s[j]:
                return False
            i += 1
            j -= 1
        return True
```

不转字符串, 没想出来，看题解。

将数字本身反转，然后将反转后的数字与原始数字进行比较，如果它们是相同的，那么这个数字就是回文。
但是，如果反转后的数字大于 \text{int.MAX}int.MAX，我们将遇到整数溢出问题。

按照第二个想法，为了避免数字反转可能导致的溢出问题，为什么不考虑只反转 \text{int}int 数字的一半？毕竟，如果该数字是回文，其后半部分反转后应该与原始数字的前半部分相同。

现在的问题是，我们如何知道反转数字的位数已经达到原始数字位数的一半？

由于整个过程我们不断将原始数字除以 10，然后给反转后的数字乘上 10，所以，当原始数字小于或等于反转后的数字时，就意味着我们已经处理了一半位数的数字了。

```python
class Solution(object):
    def isPalindrome(self, x):
        # 特殊情况：
        # 如上所述，当 x < 0 时，x 不是回文数。
        # 同样地，如果数字的最后一位是 0，为了使该数字为回文，
        # 则其第一位数字也应该是 0
        # 只有 0 满足这一属性
        if x < 0 or (x > 0 and x % 10 == 0):
            return False
        
        y = 0
        while x > y:
            y = y * 10 + x % 10
            x = x / 10
        
        # 当数字长度为奇数时，我们可以通过 revertedNumber/10 去除处于中位的数字。
        # 例如，当输入为 12321 时，在 while 循环的末尾我们可以得到 x = 12，revertedNumber = 123，
        # 由于处于中位的数字不影响回文（它总是与自己相等），所以我们可以简单地将其去除。
        return x == y or x == y/10
```