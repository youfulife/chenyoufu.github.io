---
title: "24 两两交换链表中的节点"
date: 2021-05-31T19:38:35+08:00
draft: false
tags: ["算法", "链表"]
categories: ["技术"]
---

**题目**

给定一个链表，两两交换其中相邻的节点，并返回交换后的链表。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]

输入：head = []
输出：[]

输入：head = [1]
输出：[1]
```

链接：https://leetcode-cn.com/problems/swap-nodes-in-pairs

**解题思路**

```go
func swapPairs(head *ListNode) *ListNode {

    dummy := &ListNode{
        Next: head,
    }

    prev := dummy
    cur := head

    for cur != nil {
        next := cur.Next
        if next == nil {
            break
        }

        cur.Next = next.Next
        next.Next = cur
        prev.Next = next

        prev = cur
        cur = cur.Next
    }
    return dummy.Next
}
```