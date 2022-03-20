---
title: "147 对链表进行插入排序"
date: 2021-05-29T12:46:33+08:00
draft: false
tags: ["算法", "链表", "排序"]
categories: ["技术"]
---

**题目**

插入排序算法：

插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
重复直到所有输入数据插入完为止。
 
**示例 1：**
```
输入: 4->2->1->3
输出: 1->2->3->4
```

链接：https://leetcode-cn.com/problems/insertion-sort-list

**解题思路**

```go
func insertionSortList(head *ListNode) *ListNode {
    if head == nil {
        return nil
    }

    dummy := &ListNode{
        Next: head,
    }
    last := head
    cur := head.Next
    for cur != nil {
        if cur.Val == last.Val {
            last = cur
            cur = cur.Next
            continue
        }

        prev := dummy
        for prev.Next.Val <= cur.Val {
            prev = prev.Next
        }
        
        last.Next = cur.Next
        cur.Next = prev.Next
        prev.Next = cur
        cur = last.Next
    }
    return dummy.Next
}
```

精简

```go
func insertionSortList(head *ListNode) *ListNode {
    dummy := &ListNode{ // 这里不能指定 Next = head 
    }

    cur := head
    for cur != nil {
        next := cur.Next
        prev := dummy
        for prev.Next != nil && prev.Next.Val < cur.Val {
            prev = prev.Next
        }
        
        cur.Next = prev.Next
        prev.Next = cur
        cur = next
    }
    return dummy.Next
}

```

----

```python
class Solution(object):
    # 设置虚拟头结点的意义在于，使得所有插入情况的操作具有了一致性，即使头结点变动，也能轻松获取到头结点
    # 明确出步骤： 先找它 - 临时保存它 - 删掉它 - 给它找位置 - 插入它
    def insertionSortList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        dummy = ListNode()
        dummy.next = head

        p = head
        # p.next 是需要删除的点
        while p and p.next:
            # 递增不需要处理
            if p.val <= p.next.val:
                p = p.next
                continue
            # 临时保存后删除
            tmp = p.next
            p.next = p.next.next
            # 找到插入点
            q = dummy
            while q.next and tmp.val > q.next.val:
                q = q.next
            # 插入
            tmp.next = q.next
            q.next = tmp
        return dummy.next
```

```python
class Solution(object):
    def insertionSortList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        dummy = ListNode()
        # 这里不能指定 Next = head, 相当于dummy是一个新的链表
        # dummy.next = head 

        p = head
        while p:
            q = dummy
            while q.next and p.val > q.next.val:
                q = q.next

            tmp = p.next
            p.next = q.next
            q.next = p
            p = tmp
        return dummy.next
```