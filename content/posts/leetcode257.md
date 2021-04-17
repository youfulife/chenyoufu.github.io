---
title: "二叉树的所有路径"
date: 2021-04-16T22:47:47+08:00
draft: false
---


**题目**

给定一个二叉树，返回所有从根节点到叶子节点的路径。

**示例：**

```
输入:

   1
 /   \
2     3
 \
  5

输出: ["1->2->5", "1->3"]

解释: 所有根节点到叶子节点的路径为: 1->2->5, 1->3
```

链接：https://leetcode-cn.com/problems/binary-tree-paths


**解题思路**

1. 深度优先，前序遍历，引入一个辅助函数，传递两个参数，来记录访问过的值。写的时候的注意点就是不要忘记用str转化root.val的值。

```python
class Solution(object):
    def binaryTreePaths(self, root):
        ans = []
        if root:
            self.traverse(root, "", ans)
        return ans
    
    def traverse(self, root, path, ans):
        path += str(root.val)
        if root.left is None and root.right is None:
            ans.append(path)
            return
        
        if root.left:
            self.traverse(root.left, path + "->", ans)
        if root.right:
            self.traverse(root.right, path + "->", ans)

```

**V2 精简后的代码**

```python
class Solution(object):
    def binaryTreePaths(self, root):
        """
        :type root: TreeNode
        :rtype: List[str]
        """
        ans = []
        if root:
            self.traverse(root, str(root.val), ans)
        return ans
    
    def traverse(self, root, path, ans):
        if not root.left and not root.right:
            ans.append(path)
            return
        
        if root.left:
            self.traverse(root.left, path + "->" + str(root.left.val), ans)
        if root.right:
            self.traverse(root.right, path + "->" + str(root.right.val), ans)
```

**V3 深度优先非递归**

```python
class Solution(object):
    def binaryTreePaths(self, root):
        if not root:
            return []
        
        ans = []
        stack = [(root, str(root.val))]
        
        while stack:
            node, path = stack.pop()
            if not node.left and not node.right:
                ans.append(path)

            if node.right:
                stack.append((node.right, path + "->" + str(node.right.val)))
            if node.left:
                stack.append((node.left, path + "->" + str(node.left.val)))
        return ans
```

2. 广度优先, 两个队列，同步进行

```python
class Solution(object):
    def binaryTreePaths(self, root):
        ans = []
        if not root:
            return ans

        q1 = deque()
        q1.append(root)

        q2 = deque()
        q2.append("")

        while q1:
            node = q1.popleft()
            path = str(q2.popleft())
            path += str(node.val)
            if not node.left and not node.right:
                ans.append(path)
            if node.left:
                q1.append(node.left)
                q2.append(path + "->")
            if node.right:
                q1.append(node.right)
                q2.append(path + "->")

        return ans
```

优化后的代码

```python
class Solution(object):
    def binaryTreePaths(self, root):
        ans = []
        if not root:
            return ans

        q1 = deque([root])
        q2 = deque([str(root.val)])

        while q1:
            node = q1.popleft()
            path = q2.popleft()
            if not node.left and not node.right:
                ans.append(path)
            if node.left:
                q1.append(node.left)
                q2.append(path + "->" + str(node.left.val))
            if node.right:
                q1.append(node.right)
                q2.append(path + "->" + str(node.right.val))

        return ans
```