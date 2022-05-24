---
title: "设计"
date: 2022-04-13T22:34:19+08:00
draft: false
tags: ["算法", "栈"]
categories: ["技术"]
---

[155. 最小栈](https://leetcode-cn.com/problems/min-stack/)



### 155. 最小栈

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

* push(x) —— 将元素 x 推入栈中。
* pop() —— 删除栈顶的元素。
* top() —— 获取栈顶元素。
* getMin() —— 检索栈中的最小元素。


链接：https://leetcode-cn.com/problems/min-stack

**解题思路**

* 使用一个辅助栈，与元素栈同步插入与删除，用于存储与每个元素对应的最小值。

* 当一个元素要入栈时，我们取当前辅助栈的栈顶存储的最小值，与当前元素比较得出最小值，将这个最小值插入辅助栈中；

* 当一个元素要出栈时，我们把辅助栈的栈顶元素也一并弹出；

* 在任意一个时刻，栈内元素的最小值就存储在辅助栈的栈顶元素中。

```python
class MinStack(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        self.min = []

    def push(self, val):
        """
        :type val: int
        :rtype: None
        """
        self.stack.append(val)
        if not self.min:
            self.min.append(val)
        else:
            self.min.append(min(val, self.getMin()))
        return

    def pop(self):
        """
        :rtype: None
        """
        self.stack = self.stack[:-1]
        self.min = self.min[:-1]
        return

    def top(self):
        """
        :rtype: int
        """
        return None if not self.stack else self.stack[-1]

    def getMin(self):
        """
        :rtype: int
        """
        return None if not self.min else self.min[-1]
```

**继续优化 不使用辅助栈**

栈中存当前元素和最小值的差值

```python
class MinStack(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        self.min = 0

    def push(self, x):
        """
        :type x: int
        :rtype: None
        """
        if not self.stack:
            self.min = x
        diff = x - self.min
        if diff < 0:
            self.min = x
        self.stack.append(diff)


    def pop(self):
        """
        :rtype: None
        """
        diff = self.stack[-1]
        if diff < 0:
            self.min = self.min - diff
        self.stack.pop()


    def top(self):
        """
        :rtype: int
        """
        diff = self.stack[-1]
        if diff < 0:
            return self.min
        else:
            return self.min + diff


    def getMin(self):
        """
        :rtype: int
        """
        return self.min
```