---
title: "二叉树层序遍历"
date: 2021-04-13T22:31:09+08:00
draft: false
---

**题目**

给你一个二叉树，请你返回其按 层序遍历 得到的节点值。 （即逐层地，从左到右访问所有节点）。

**示例：**

二叉树：[3,9,20,null,null,15,7],

```
    3
   / \
  9  20
    /  \
   15   7
```
返回其层序遍历结果：
```
[
  [3],
  [9,20],
  [15,7]
]
```

链接：https://leetcode-cn.com/problems/binary-tree-level-order-traversal


**解题思路**

1. Using BFS, at any instant only L1 and L1+1 nodes are in the queue.
2. When we start the while loop, we have L1 nodes in the queue.
3. for _ in range(len(q)) allows us to dequeue L1 nodes one by one and add L2 children one by one.
4. Time complexity: O(N). Space Complexity: O(N)

V1 自己写的

```python
class Solution(object):
    def levelOrder(self, root):
        ret = []
        if root is None:
            return ret
        
        q = deque()
        q.append(root)
        while q:
            size = len(q)
            level = []
            for i in range(size):
                node = q.popleft()
                level.append(node.val)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            ret.append(level)
        return ret
```

V2 优化后的代码

```python
class Solution(object):
    def levelOrder(self, root):
        result = []
        q = deque()
        if root:
            q.append(root)
        while q:
            level = []
            for _ in range(len(q)):
                node = q.popleft()
                level.append(node.val)
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            else:
                result.append(level)
        return result
```

V3 深度优先 递归

1. Use a variable to track level in the tree and use simple Pre-Order traversal
2. Add sub-lists to result as we move down the levels
3. Time Complexity: O(N)
4. Space Complexity: O(N) + O(h) for stack space

```python
class Solution(object):
    def levelOrder(self, root):
        result = []
        self.helper(root, 0, result)
        return result
    
    def helper(self, root, level, result):
        if root is None:
            return
        
        if level >= len(result):
            result.append([])
        
        result[level].append(root.val)
        self.helper(root.left, level+1, result)
        self.helper(root.right, level+1, result)
```

关于层序遍历的题目

* 102.二叉树的层序遍历

* 107.二叉树的层次遍历II

* 199.二叉树的右视图

* 637.二叉树的层平均值

* 589.N叉树的前序遍历

* 513.找树左下角的值