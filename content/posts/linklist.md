---
title: "链表系列"
date: 2022-02-09T22:31:30+08:00
draft: false
tags: ["算法", "链表"]
categories: ["技术"]
---

### 总结

1. 在调用 next 字段之前，始终检查节点是否为空。

获取空节点的下一个节点将导致空指针错误。例如，在我们运行 fast = fast.next.next 之前，需要检查 fast 和 fast.next 不为空。

2. 仔细定义循环的结束条件。

运行几个示例，以确保你的结束条件不会导致无限循环。尤其是设置尾巴为None，否则容易形成环。

很多时候需要while cur and cur.next 作为条件


3. 你可以同时使用多个指针。

有时，当你为链表问题设计算法时，可能需要同时跟踪多个结点。您应该记住需要跟踪哪些结点，并且可以自由地使用几个不同的结点指针来同时跟踪这些结点。

如果你使用多个指针，最好为它们指定适当的名称，以防将来必须调试或检查代码。

 

4. 在许多情况下，你需要跟踪当前结点的前一个结点。

你无法追溯单链表中的前一个结点。因此，您不仅要存储当前结点，还要存储前一个结点。这在双链表中是不同的，我们将在后面的章节中介绍。

尤其是在删除节点的时候，需要dummy节点，最后返回dummy.next。


**题目**

* [707. 设计链表](https://leetcode-cn.com/problems/design-linked-list)
* [141. 环形链表](https://leetcode-cn.com/problems/linked-list-cycle/)
* [142. 环形链表 II](https://leetcode-cn.com/problems/linked-list-cycle-ii)
* [160. 相交链表](https://leetcode-cn.com/problems/intersection-of-two-linked-lists)
* [19. 删除链表的倒数第 N 个结点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list)
* [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list)
* [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements)
* [328. 奇偶链表](https://leetcode-cn.com/problems/odd-even-linked-list)
* [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list)
* [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)
* [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers)
* [138. 复制带随机指针的链表](https://leetcode-cn.com/leetbook/read/linked-list/fw8v5/)
* [430.扁平化多级双向链表](https://leetcode-cn.com/leetbook/read/linked-list/fw8v5/)

### 707. 设计链表

设计链表的实现。您可以选择使用单链表或双链表。单链表中的节点应该具有两个属性：val 和 next。val 是当前节点的值，next 是指向下一个节点的指针/引用。如果要使用双向链表，则还需要一个属性 prev 以指示链表中的上一个节点。假设链表中的所有节点都是 0-index 的。

在链表类中实现这些功能：

* get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
* addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
* addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
* addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val  的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
* deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/design-linked-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Node(object):
    def __init__(self, val):
        self.val = val
        self.next = None

class MyLinkedList(object):

    def __init__(self):
        self.size = 0
        self.head = Node(0)
        


    def get(self, index):
        """
        :type index: int
        :rtype: int
        """
        if index < 0 or index >= self.size:
            return -1
        prev = self.head
        for i in range(index):
            prev = prev.next
        return prev.next.val

    def addAtHead(self, val):
        """
        :type val: int
        :rtype: None
        """
        self.addAtIndex(0, val)


    def addAtTail(self, val):
        """
        :type val: int
        :rtype: None
        """
        self.addAtIndex(self.size, val)


    def addAtIndex(self, index, val):
        """
        :type index: int
        :type val: int
        :rtype: None
        """
        if index > self.size:
            return -1
        if index < 0:
            index = 0
        prev = self.head
        for _ in range(index):
            prev = prev.next
        
        cur = Node(val)
        cur.next = prev.next
        prev.next = cur
        self.size += 1

    def deleteAtIndex(self, index):
        """
        :type index: int
        :rtype: None
        """

        if index< 0 or index >= self.size:
            return
        
        prev = self.head
        for _ in range(index):
            prev = prev.next
        prev.next = prev.next.next
        self.size -= 1

# Your MyLinkedList object will be instantiated and called as such:
# obj = MyLinkedList()
# param_1 = obj.get(index)
# obj.addAtHead(val)
# obj.addAtTail(val)
# obj.addAtIndex(index,val)
# obj.deleteAtIndex(index)
```

### 141. 环形链表

给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。

**解题思路**

快慢指针

```python
class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        slow = head
        fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        return False
```

### 142. 环形链表 II

给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/linked-list-cycle-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def detectCycle(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        slow = head
        fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                slow = head
                while slow != fast:
                    slow = slow.next
                    fast = fast.next
                return slow
        return None
```

### 160. 相交链表

给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 null 。

题目数据 保证 整个链式结构中不存在环。

注意，函数返回结果后，链表必须 保持其原始结构 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/intersection-of-two-linked-lists
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def getIntersectionNode(self, headA, headB):
        """
        :type head1, head1: ListNode
        :rtype: ListNode
        """
        if not headA and not headB:
            return None
        x = headA
        y = headB
        while x != y:
            if not x:
                x = headB
            else:
                x = x.next
            if not y:
                y = headA
            else:
                y = y.next
        return x
```

### 19. 删除链表的倒数第 N 个结点

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        dummy = ListNode(0, head)
        fast = dummy
        for _ in range(n):
            fast = fast.next

        slow = dummy
        while fast.next:
            slow = slow.next
            fast = fast.next
        
        slow.next = slow.next.next
        return dummy.next
```

### 206. 反转链表

给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。

提示：

链表中节点的数目范围是 [0, 5000]
-5000 <= Node.val <= 5000
 

进阶：链表可以选用迭代或递归方式完成反转。你能否用两种方法解决这道题？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/reverse-linked-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        prev = None
        cur = head
        
        while cur:
            n = cur.next
            cur.next = prev
            prev = cur
            cur = n
        
        return prev
```

```python
class Solution(object):
    def reverseList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if not head or not head.next:
            return head
        
        newhead = self.reverseList(head.next)
        head.next.next = head
        head.next = None # 万不可忘记
        return newhead
```

### 203. 移除链表元素

给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/remove-linked-list-elements
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def removeElements(self, head, val):
        """
        :type head: ListNode
        :type val: int
        :rtype: ListNode
        """
        dummy = ListNode(0, head)
        prev = dummy
        while prev.next:
            if prev.next.val == val:
                prev.next = prev.next.next
            else:
                prev = prev.next
            
        return dummy.next
```

### 328. 奇偶链表

给定单链表的头节点 head ，将所有索引为奇数的节点和索引为偶数的节点分别组合在一起，然后返回重新排序的列表。

第一个节点的索引被认为是 奇数 ， 第二个节点的索引为 偶数 ，以此类推。

请注意，偶数组和奇数组内部的相对顺序应该与输入时保持一致。

你必须在 O(1) 的额外空间复杂度和 O(n) 的时间复杂度下解决这个问题。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/odd-even-linked-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```python
class Solution(object):
    def oddEvenList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if not head or not head.next:
            return head
        oddTail, evenTail = head, head.next
        evenHead = head.next
        while evenTail and evenTail.next:
            oddTail.next = evenTail.next
            oddTail = oddTail.next
            evenTail.next = oddTail.next
            evenTail = evenTail.next
        oddTail.next = evenHead
        return head
```

### 234. 回文链表

给你一个单链表的头节点 head ，请你判断该链表是否为回文链表。如果是，返回 true ；否则，返回 false 。

提示：

链表中节点数目在范围[1, 105] 内
0 <= Node.val <= 9
 

进阶：你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/palindrome-linked-list
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def isPalindrome(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        arr = []
        cur = head
        while cur:
            arr.append(cur.val)
            cur = cur.next
        return arr == arr[::-1]
```

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution(object):
    def isPalindrome(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        # 快慢指针找到中间点
        slow = head
        fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        
        # 反转后半部分
        prev = None
        cur = slow
        while cur:
            n = cur.next
            cur.next = prev
            prev = cur
            cur = n
        
        # 判断回文
        left = head
        right = prev
                
        while left and right:
            if left.val != right.val:
                return False
            left = left.next
            right = right.next
        return True
```

### 21. 合并两个有序链表

将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

提示：

两个链表的节点数目范围是 [0, 50]
-100 <= Node.val <= 100
l1 和 l2 均按 非递减顺序 排列

```python
class Solution(object):
    def mergeTwoLists(self, list1, list2):
        """
        :type list1: Optional[ListNode]
        :type list2: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        dummy = ListNode()

        l1 = list1
        l2 = list2
        cur = dummy
        while l1 and l2:
            if l1.val >= l2.val:
                cur.next = l2
                l2 = l2.next
            else:
                cur.next = l1
                l1 = l1.next
            cur = cur.next
        if l1:
            cur.next = l1
        if l2:
            cur.next = l2

        return dummy.next
```

```python
class Solution(object):
    def mergeTwoLists(self, list1, list2):
        """
        :type list1: Optional[ListNode]
        :type list2: Optional[ListNode]
        :rtype: Optional[ListNode]
        """
        if not list1:
            return list2
        if not list2:
            return list1
        
        if list1.val >= list2.val:
            list2.next = self.mergeTwoLists(list1, list2.next)
            return list2
        else:
            list1.next = self.mergeTwoLists(list1.next, list2)
            return list1
```

### 2. 两数相加

给你两个 非空 的链表，表示两个非负的整数。它们每位数字都是按照 逆序 的方式存储的，并且每个节点只能存储 一位 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/add-two-numbers
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


如果两个链表的长度不同，则可以认为长度短的链表的后面有若干个 0。

此外，如果链表遍历结束后，有 carry>0，还需要在答案链表的后面附加一个节点，节点的值为 carry。

```python
class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        p = l1
        n1 = 0
        i = 0
        while p:
            n1 += p.val * (10 ** i)
            i += 1
            p = p.next
        
        p = l2
        n2 = 0
        i = 0
        while p:
            n2 += p.val * (10 ** i)
            p = p.next
            i += 1
        
        s = n1 + n2
        if s == 0:
            return ListNode(0)

        h = ListNode(0)
        cur = h
        while s:
            v = s % 10
            s = s / 10
            cur.next = ListNode(v)
            cur = cur.next
        return h.next
```

正规解法

```python
class Solution(object):
    def addTwoNumbers(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        head = ListNode(0)
        cur = head
        c = 0
        while l1 or l2:
            v1 = l1 and l1.val or 0
            v2 = l2 and l2.val or 0
            s = v1 + v2 + c
            v = s % 10
            c = s / 10
            cur.next = ListNode(v)
            cur = cur.next
            if l1:
                l1 = l1.next
            if l2:
                l2 = l2.next

        if c > 0:
            cur.next = ListNode(c)
        
        return head.next
```

### 138. 复制带随机指针的链表

给你一个长度为 n 的链表，每个节点包含一个额外增加的随机指针 random ，该指针可以指向链表中的任何节点或空节点。

构造这个链表的 深拷贝。 深拷贝应该正好由 n 个 全新 节点组成，其中每个新节点的值都设为其对应的原节点的值。新节点的 next 指针和 random 指针也都应指向复制链表中的新节点，并使原链表和复制链表中的这些指针能够表示相同的链表状态。复制链表中的指针都不应指向原链表中的节点 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/copy-list-with-random-pointer
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
"""
# Definition for a Node.
class Node:
    def __init__(self, x, next=None, random=None):
        self.val = int(x)
        self.next = next
        self.random = random
"""

class Solution(object):
    def copyRandomList(self, head):
        """
        :type head: Node
        :rtype: Node
        """
        if not head:
            return None
        src = head
        dummy = Node(0)
        dst = dummy
        
        m = {}
        while src:
            dst.next = Node(src.val)
            dst = dst.next
            m[src] = dst
            src = src.next
            
        src = head
        dst = dummy.next
        while dst and src:
            dst.random = m.get(src.random, None)
            src = src.next
            dst = dst.next

        return dummy.next
```

### 430.扁平化多级双向链表

你会得到一个双链表，其中包含的节点有一个下一个指针、一个前一个指针和一个额外的 子指针 。这个子指针可能指向一个单独的双向链表，也包含这些特殊的节点。这些子列表可以有一个或多个自己的子列表，以此类推，以生成如下面的示例所示的 多层数据结构 。

作者：力扣 (LeetCode)
链接：https://leetcode-cn.com/leetbook/read/linked-list/fw8v5/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```python
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val, prev, next, child):
        self.val = val
        self.prev = prev
        self.next = next
        self.child = child
"""

class Solution(object):
    def flatten(self, head):
        """
        :type head: Node
        :rtype: Node
        """
        cur = head
        while cur:
            next = cur.next
            if cur.child:
                h = self.flatten(cur.child)
                
                cur.child.prev = cur
                cur.next = cur.child
                while h and h.next:
                    h = h.next
                h.next = next
                if next:
                    next.prev = h
            cur.child = None
            cur = next
        return head
```