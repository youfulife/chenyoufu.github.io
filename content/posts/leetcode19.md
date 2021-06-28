---
title: "19 删除链表的倒数第 N 个结点"
date: 2021-06-28T22:10:33+08:00
draft: false
tags: ["算法", "链表"]
categories: ["技术"]
---
**题目**
给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

进阶：你能尝试使用一趟扫描实现吗？

**解题思路**

双指针，一遍扫描，找到要删除节点的前一个节点，然后删除即可。

注意边界情况。

```python
class Solution(object):
    def removeNthFromEnd(self, head, n):
        # 1->nil, 1, nil
        # 1->2->3->4->5->nil, 2, 1->2->3->5->nil
        dummy = ListNode(next=head)
        first = head
        for i in range(n):
            first = first.next
        
        second = dummy
        while first:
            first = first.next
            second = second.next
        
        second.next = second.next.next
        return dummy.next
```