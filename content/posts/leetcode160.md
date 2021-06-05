---
title: "160 相交链表"
date: 2021-06-04T22:35:49+08:00
draft: false
tags: ["算法", "链表"]
categories: ["技术"]
---
**题目**

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。

题目数据 保证 整个链式结构中不存在环。

注意，函数返回结果后，链表必须 保持其原始结构 。

链接：https://leetcode-cn.com/problems/intersection-of-two-linked-lists

**解题思路**


```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {
    
    p := headA
    q := headB
    isFirstP := true // 放两个标志位，表示第一次遍历
    isFirstQ := true

    for  p != nil && q != nil {
        if p == q {
            return p
        }
        
         p = p.Next
         if p == nil && isFirstP { // 最后一个节点，那么就指向第二个的头节点。
             p = headB
             isFirstP = false // 这里忘记赋值，导致死循环。
         }

        q = q.Next
        if q == nil && isFirstQ {
            q = headA
            isFirstQ = false
        }
    }

    return nil    
}
```

其实上面逻辑明显复杂。

```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {

    if headA == nil || headB == nil {
        return nil
    }

    p, q := headA, headB
    for p != q {
        if p == nil {
            p = headB
        } else {
            p = p.Next
        }

        if q == nil {
            q = headA
        } else {
            q = q.Next
        }
    }

    return p
}
```