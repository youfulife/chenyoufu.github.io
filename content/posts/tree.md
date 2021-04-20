---
title: "树"
date: 2021-04-12T22:22:08+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

一直对树有一种畏惧感，主要是平时用的不多，自己又学的不好，在这之前本身就有一种逃避的心态。

所有的恐惧都是源于自己的无知，关于树需要来一场刻意练习，摆正心态，拉长战线，彻底消灭这个心魔。

先从最基本的二叉树树的遍历开始。

5种遍历 * 递归+非递归，熟练记忆并且写成肌肉记忆，达到不加思考直接写的能力。

**前序遍历（递归）**

根 ➜ 左 ➜ 右

```python
class Solution(object):
    def preorderTraversal(self, root):
        ans = []
        self.helper(root, ans)
        return ans
    
    def helper(self, root, ans):
        if not root:
            return
        ans.append(root.val)
        self.helper(root.left, ans)
        self.helper(root.right, ans)
```

**前序遍历（非递归）**

先处理根节点，根据访问顺序根 ➜ 左 ➜ 右，先入栈的后访问，为了保持访问顺序（先入后出），⭐️先把右孩子入栈，再入栈左孩子。

```python
class Solution(object):
    def preorderTraversal(self, root):
        if not root:
            return []
        
        ans = []
        q = deque([root])
        while q:
            node = q.pop()
            ans.append(node.val)
            if node.right:
                q.append(node.right)
            if node.left:
                q.append(node.left)

        return ans
```

**中序遍历（递归）**

```python
class Solution(object):
    def inorderTraversal(self, root):
        ans = []
        self.helper(root, ans)
        return ans
    
    def helper(self, root, ans):
        if root:
            self.helper(root.left, ans)
            ans.append(root.val)
            self.helper(root.right, ans)
```

**中序遍历（非递归）**

核心思路依旧是利用栈维护节点的访问顺序：左 ➜ 根 ➜ 右。使用一个p_node来指向当前访问节点，p是代表指针point，另外有一个变量cur_node表示当前正在操作节点（把出栈节点值加入输出数组中），算法步骤如下（可以对照代码注释）

① 访问当前节点，如果当前节点有左孩子，则把它的左孩子都入栈，移动当前节点到左孩子，重复第一步直到当前节点没有左孩子

② 当当前节点没有左孩子时，栈顶节点出栈，加入结果数组

③ 当前节点指向栈顶节点的右节点

```python
class Solution(object):
    def inorderTraversal(self, root):
        if not root:
            return []
        
        ans = []
        q = deque()
        
        cur = root
        while cur or q:
            while cur: # 把所有当前访问节点的左孩子都入栈
                q.append(cur)
                cur = cur.left
            
            node = q.pop() # 操作栈顶节点，如果是第一次运行到这步，那么这是整棵树的最左节点
            ans.append(node.val) # 因为已经保证没有左节点，可以访问根节点
            if node.right:
                cur = node.right # 将指针指向当前节点的右节点
        return ans
```

**后序遍历（递归）**

```python
class Solution(object):
    def postorderTraversal(self, root):
        ans = []
        self.helper(root, ans)
        return ans
    
    def helper(self, root, ans):
        if root:
            self.helper(root.left, ans)
            self.helper(root.right, ans)
            ans.append(root.val)
```

**后序遍历（非递归）**

**深度遍历（递归）**

**深度遍历（非递归）**

**广度遍历（递归）**

**广度遍历（非递归）**

----

2021.4.17 更新 一周总结

第一个周熟悉了二叉树的各种遍历方式，很多题目都依赖二叉树所有层遍历和所有路径遍历，基本上都是在这两个的基础上做一些简单的运算。比如求某一层的平均值，最大值，最左边的值，右下角的值等。

所以有两个题目必须写到肌肉记忆，就是输出二叉树每一层的节点和每一条路径的节点，每隔两天就要写一下递归和非递归的方法，防止遗忘。

