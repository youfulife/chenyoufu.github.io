---
title: "237 删除链表中的节点"
date: 2021-06-28T21:59:42+08:00
draft: false
tags: ["算法", "链表"]
categories: ["技术"]
---
**题目**

请编写一个函数，使其可以删除某个链表中给定的（非末尾）节点。传入函数的唯一参数为 要被删除的节点 。

**解题思路**

没有给定头节点，所以只能把要删除节点后面的值赋值过来，然后删除next节点。

```python
class Solution(object):
    def deleteNode(self, node):
        node.val = node.next.val
        node.next = node.next.next
```