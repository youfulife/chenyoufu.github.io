---
title: "剑指 Offer 22 链表中倒数第k个节点"
date: 2021-05-30T08:08:11+08:00
draft: false
tags: ["算法", "链表"]
categories: ["技术"]
---

**题目**

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。

**示例：**

```
给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.
```

链接：https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof

**解题思路**

1. 两次遍历，先把长度求出来，然后再从头遍历到合适的位置。

```go
func getKthFromEnd(head *ListNode, k int) *ListNode {
    l := 0
    
    cur := head
    for cur != nil {
        cur = cur.Next
        l++
    }

    cur = head
    for i:=0; i<l-k; i++ {
        cur = cur.Next
    }

    return cur
}
```

2. 双指针，1次遍历。

```go
func getKthFromEnd(head *ListNode, k int) *ListNode {
    first := head
    second := head

    for i:=0;i<k;i++ { // 注意边界条件
        second = second.Next
    }

    for second != nil {
        first = first.Next
        second = second.Next
    }
    return first
}

```

