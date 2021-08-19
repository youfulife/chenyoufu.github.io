---
title: "剑指 Offer 52 两个链表的第一个公共节点"
date: 2021-05-29T20:46:39+08:00
draft: false
tags: ["算法", "链表", "第一个"]
categories: ["技术"]
---

**题目**

输入两个链表，找出它们的第一个公共节点。

**解题思路**

我们使用两个指针 node1，node2 分别指向两个链表 headA，headB 的头结点，然后同时分别逐结点遍历，当 node1 到达链表 headA 的末尾时，重新定位到链表 headB 的头结点；当 node2 到达链表 headB 的末尾时，重新定位到链表 headA 的头结点。

这样，当它们相遇时，所指向的结点就是第一个公共结点。

```go
func getIntersectionNode(headA, headB *ListNode) *ListNode {

    if headA == nil || headB == nil {
        return nil
    }
    
    p := headA
    q := headB
    
    for p != q {
        // 边界一定要确定好
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