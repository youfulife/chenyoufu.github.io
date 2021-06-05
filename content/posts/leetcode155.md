---
title: "155 最小栈"
date: 2021-06-05T10:34:19+08:00
draft: false
tags: ["算法", "栈"]
categories: ["技术"]
---

**题目**

设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。

* push(x) —— 将元素 x 推入栈中。
* pop() —— 删除栈顶的元素。
* top() —— 获取栈顶元素。
* getMin() —— 检索栈中的最小元素。


链接：https://leetcode-cn.com/problems/min-stack

**解题思路**

每次插入或者删除的时候，维护一个有序的数组。

```python
class MinStack(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        self.min = []

    def push(self, val):
        """
        :type val: int
        :rtype: None
        """
        self.stack.append(val)
        self.min.append(val)
        self.min.sort()
        
    def pop(self):
        """
        :rtype: None
        """
        val = self.top()
        self.stack = self.stack[:-1]
        for i in range(len(self.min)):
            if self.min[i] == val:
                self.min = self.min[:i] + self.min[i+1:]
                break
        return val


    def top(self):
        """
        :rtype: int
        """
        if len(self.stack) > 0:
            return self.stack[-1]
        return None


    def getMin(self):
        """
        :rtype: int
        """
        if len(self.min) > 0:
            return self.min[0]
        else:
            return None
```

**优化**

想的复杂了, 其实不需要排序，每次插入和删除，遍历一遍所有元素，只要保存一个最小值就行了。

```python
class MinStack(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        self.min = None

    def push(self, val):
        """
        :type val: int
        :rtype: None
        """
        self.stack.append(val)
        self.min = min(self.stack)
        
    def pop(self):
        """
        :rtype: None
        """
        val = self.top()
        self.stack = self.stack[:-1]
        self.min = min(self.stack) if len(self.stack) > 0 else None
        return val

    def top(self):
        """
        :rtype: int
        """
        if len(self.stack) > 0:
            return self.stack[-1]
        return None



    def getMin(self):
        """
        :rtype: int
        """
        return self.min
```

**继续优化**

* 使用一个辅助栈，与元素栈同步插入与删除，用于存储与每个元素对应的最小值。

* 当一个元素要入栈时，我们取当前辅助栈的栈顶存储的最小值，与当前元素比较得出最小值，将这个最小值插入辅助栈中；

* 当一个元素要出栈时，我们把辅助栈的栈顶元素也一并弹出；

* 在任意一个时刻，栈内元素的最小值就存储在辅助栈的栈顶元素中。

```python
class MinStack(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        self.min = []

    def push(self, val):
        """
        :type val: int
        :rtype: None
        """
        self.stack.append(val)
        if not self.min:
            self.min.append(val)
        else:
            self.min.append(min(val, self.getMin()))
        return

    def pop(self):
        """
        :rtype: None
        """
        self.stack = self.stack[:-1]
        self.min = self.min[:-1]
        return

    def top(self):
        """
        :rtype: int
        """
        return None if not self.stack else self.stack[-1]

    def getMin(self):
        """
        :rtype: int
        """
        return None if not self.min else self.min[-1]
```

**继续优化**

如果只允许用一个栈，那么每次push 一个 (val, min) 元组进去。

**基于链表实现**

国际站最高赞答案。

```java
class MinStack {
    private Node head;
    
    public void push(int x) {
        if(head == null) 
            head = new Node(x, x);
        else 
            head = new Node(x, Math.min(x, head.min), head);
    }

    public void pop() {
        head = head.next;
    }

    public int top() {
        return head.val;
    }

    public int getMin() {
        return head.min;
    }
    
    private class Node {
        int val;
        int min;
        Node next;
        
        private Node(int val, int min) {
            this(val, min, null);
        }
        
        private Node(int val, int min, Node next) {
            this.val = val;
            this.min = min;
            this.next = next;
        }
    }
}
```