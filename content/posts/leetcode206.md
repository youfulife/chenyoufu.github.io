---
title: "206 反转链表"
date: 2021-05-30T08:18:21+08:00
draft: false
tags: ["算法", "链表"]
categories: ["技术"]
---

**题目**

给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**解题思路**

自己写的

```go
func reverseList(head *ListNode) *ListNode {
    if head == nil {
        return head
    }

    dummy := &ListNode{
        Next: head,
    }

    cur := head.Next

    for cur != nil {
        next := cur.Next

        head.Next = cur.Next
        cur.Next = dummy.Next
        dummy.Next = cur

        cur = next
    }

    return dummy.Next
}
```

看官方提交优化后

```go
func reverseList(head *ListNode) *ListNode {
    var prev *ListNode
    cur := head
    
    for cur != nil {
        next := cur.Next

        cur.Next = prev
        prev = cur
        cur = next
    }
    return prev
}
```

递归

```go
func reverseList(head *ListNode) *ListNode {
    if head == nil || head.Next == nil {
        return head
    }

    newHead :=  reverseList(head.Next)
    head.Next.Next = head
    head.Next = nil

    return newHead
}
```