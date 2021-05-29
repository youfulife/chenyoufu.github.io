---
title: "21 合并两个有序链表"
date: 2021-05-29T08:59:45+08:00
draft: false
tags: ["算法", "链表"]
categories: ["技术"]
---

**题目**

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

```
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
```

示例 2：

```
输入：l1 = [], l2 = []
输出：[]
```

示例 3：

```
输入：l1 = [], l2 = [0]
输出：[0]
```

链接：https://leetcode-cn.com/problems/merge-two-sorted-lists

**解题思路**

两个指针分别指向要合并的链表，从头到尾遍历比较大小。

```go

type ListNode struct {
  Val int
  Next *ListNode
}

func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {

  // 这两个非空判断其实可以都不要
  if l1 == nil {
    return l2
  }

  if l2 == nil {
    return l1
  }

  head := &ListNode {}
  cur := head

  p1 := l1
  p2 := l2
  for p1 != nil && p2 != nil {
    if p1.Val < p2.Val {
      cur.Next = p1
      p1 = p1.Next
    } else {
      cur.Next = p2
      p2 = p2.Next
    }
    cur = cur.Next
  }

  if p1 == nil {
    cur.Next = p2
  }

  if p2 == nil {
    cur.Next = p1
  }

  return head.Next
}
```

**递归**

```go
func mergeTwoLists(l1 *ListNode, l2 *ListNode) *ListNode {
    
    if l1 == nil {
        return l2
    }

    if l2 == nil {
        return l1
    }

    if l1.Val < l2.Val {
        l1.Next = mergeTwoLists(l1.Next, l2)
        return l1
    } else {
        l2.Next = mergeTwoLists(l1, l2.Next)
        return l2
    }
}

```