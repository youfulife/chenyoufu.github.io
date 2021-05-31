---
title: "142 环形链表 II"
date: 2021-05-30T21:57:35+08:00
draft: true
---

**题目**

给定一个链表，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意，pos 仅仅是用于标识环的情况，并不会作为参数传递到函数中。

说明：不允许修改给定的链表。

进阶：

你是否可以使用 O(1) 空间解决此题？

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

链接：https://leetcode-cn.com/problems/linked-list-cycle-ii

**解题思路**

**哈希表**

```go
func detectCycle(head *ListNode) *ListNode {
    var m = make(map[*ListNode]struct{})
    cur := head
    for cur != nil {
        if _, ok := m[cur]; ok {
            return cur
        }
        m[cur] = struct{}{}
        cur = cur.Next
    }
    return nil
}
```

**快慢指针**

```go
func detectCycle(head *ListNode) *ListNode {
    slow := head
    fast := head
    for slow != nil && fast != nil && fast.Next != nil {
        slow = slow.Next
        fast = fast.Next.Next

        if slow == fast {
            p := head
            for p != slow {
                p = p.Next
                slow = slow.Next
            }
            return p
        }
    }

    return nil
}
```