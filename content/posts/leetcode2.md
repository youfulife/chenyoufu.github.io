---
title: "2 两数相加"
date: 2021-06-07T22:16:34+08:00
draft: false
tags: ["算法", "链表"]
categories: ["技术"]
---

**题目**

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

示例 1：
```
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

示例 2：
```
输入：l1 = [0], l2 = [0]
输出：[0]
```

链接：https://leetcode-cn.com/problems/add-two-numbers

**解题思路**

对于链表问题，返回结果为头结点时，通常需要先初始化一个预先指针 pre，该指针的下一个节点指向真正的头结点head。使用预先指针的目的在于链表初始化时无可用节点值，而且链表构造过程需要指针移动，进而会导致头指针丢失，无法返回结果。

```go
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {

    p1 := l1
    p2 := l2

    flag := 0

    var l3 = &ListNode{}
    p3 := l3

    for p1 != nil && p2 != nil {

        n := &ListNode{}
        
        val := p1.Val + p2.Val + flag
        flag =  val / 10
        
        n.Val = val % 10

        p3.Next = n
        p3 = n
        
        p1 = p1.Next
        p2 = p2.Next
    }

    var p *ListNode
    if p1 != nil {
        p = p1
    } else {
        p = p2
    }

    for p != nil {
        n := &ListNode{}

        val := p.Val + flag
        flag =  val / 10

        n.Val = val % 10
        
        p3.Next = n
        p3 = n

        p = p.Next
    }

    if flag == 1 {
        p3.Next = &ListNode{
            Val: 1,
        }
    }

    return l3.Next
}
```

**优化**

如果两个链表的长度不同，则可以认为长度短的链表的后面有若干个 0。

此外，如果链表遍历结束后，有 carry>0，还需要在答案链表的后面附加一个节点，节点的值为 carry。

```go
func addTwoNumbers(l1, l2 *ListNode) (head *ListNode) {
    var tail *ListNode
    carry := 0
    for l1 != nil || l2 != nil {
        n1, n2 := 0, 0
        if l1 != nil {
            n1 = l1.Val
            l1 = l1.Next
        }
        if l2 != nil {
            n2 = l2.Val
            l2 = l2.Next
        }
        sum := n1 + n2 + carry
        sum, carry = sum%10, sum/10
        if head == nil {
            head = &ListNode{Val: sum}
            tail = head
        } else {
            tail.Next = &ListNode{Val: sum}
            tail = tail.Next
        }
    }
    if carry > 0 {
        tail.Next = &ListNode{Val: carry}
    }
    return
}
```