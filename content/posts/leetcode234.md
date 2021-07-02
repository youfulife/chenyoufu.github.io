---
title: "234 回文链表"
date: 2021-07-01T22:58:44+08:00
draft: false
tags: ["算法", "链表"]
categories: ["技术"]
---
**题目**
请判断一个链表是否为回文链表。

示例 1:
```
输入: 1->2
输出: false
```
示例 2:
```
输入: 1->2->2->1
输出: true
```
进阶：
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

链接：https://leetcode-cn.com/problems/palindrome-linked-list

**解题思路**

遍历一遍链表，将值放到一个数组中，然后判断。

```python
class Solution(object):
    def isPalindrome(self, head):
        l = []
        while head:
            l.append(head.val)
            head = head.next
        left = 0
        right = len(l)-1
        while left < right:
            if l[left] != l[right]:
                return False
            left += 1
            right -= 1
        return True
```

**反转后半部分链表，快慢指针**

```python
class Solution(object):
    def isPalindrome(self, head):
        fast = head
        slow = head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
        
        prev = None
        cur = slow
        while cur:
            next = cur.next
            cur.next = prev
            prev = cur
            cur = next
        
        while prev and head:
            if prev.val != head.val:
                return False
            prev = prev.next
            head = head.next
        
        return True
```