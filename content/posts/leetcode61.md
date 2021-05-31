---
title: "61. 旋转链表"
date: 2021-05-29T23:31:03+08:00
draft: false
tags: ["算法", "链表"]
categories: ["技术"]
---

**题目**
给你一个链表的头节点 head ，旋转链表，将链表每个节点向右移动 k 个位置。

```
输入：head = [1,2,3,4,5], k = 2
输出：[4,5,1,2,3]

输入：head = [0,1,2], k = 4
输出：[2,0,1]
```

**解题思路**

```go
func rotateRight(head *ListNode, k int) *ListNode {

    if head == nil {
        return head
    }

    dummy := &ListNode{
        Next: head,
    }

    count := 0
    cur := dummy
    for cur.Next != nil {
        cur = cur.Next
        count++
    }
    last := cur

    pos := count - k % count
    cur = dummy
    count = 0
    for cur.Next != nil {
        cur = cur.Next
        count++
        if count == pos {
            break
        }
    }

    last.Next = head
    head = cur.Next
    cur.Next = nil

    return head
}
```

**优化精简**

```go
func rotateRight(head *ListNode, k int) *ListNode {
    if head == nil {
        return head
    }

    count := 1
    cur := head
    for cur.Next != nil {
        count++
        cur = cur.Next
    }
    cur.Next = head

    pos := count - k % count // 断开的位置，从 1 开始计数
    
    for i:=0; i < pos; i++ { // 不满足条件时正好cur指向新的尾节点
        cur = cur.Next
    }

    head = cur.Next
    cur.Next = nil

    return head
}
```