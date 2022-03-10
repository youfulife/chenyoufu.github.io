---
title: "二叉树系列"
date: 2022-02-19T16:56:59+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

### 总结

* 树的遍历，递归 + 非递归，前中后层
* 树的构建，序列化
* 树的各种题型，深度，路径和，对称，反转，公共祖先，右视图
* 二叉搜索树

### 经典题目


### 二叉树非递归遍历统一模版，颜色标记法

```python
class Solution(object):
    def inorderTraversal(self, root):
        if not root:
            return []
        ans = []
        
        stack = [(root, 0)]
        while stack:
            root, color = stack.pop()
            if color == 1:
                ans.append(root.val)
            else:
                # 中序
                if root.right:
                    stack.append((root.right, 0))
                stack.append((root, 1))
                if root.left:
                    stack.append((root.left, 0))
                # 先序
                # if root.right:
                #     stack.append((root.right, 0))
                # if root.left:
                #     stack.append((root.left, 0))
                # stack.append((root, 1))

                # 后序
                # stack.append((root, 1))
                # if root.right:
                #     stack.append((root.right, 0))
                # if root.left:
                #     stack.append((root.left, 0))
        return ans
```

用两种颜色，表示节点是不是需要被处理还是只是经过，不同的顺序只需要按照定义调整这三行的顺序即可，和递归的写法类似。

```python
stack.append((root.right, 0))
stack.append((root, 1))
stack.append((root.left, 0))
```


* [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)


### 199. 二叉树的右视图

给定一个二叉树的 根节点 root，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。 

提示:

二叉树的节点个数的范围是 [0,100]
-100 <= Node.val <= 100 

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/binary-tree-right-side-view
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**解题思路**

BFS

```python
class Solution(object):
    def rightSideView(self, root):
        if not root:
            return []
        
        ans = []
        q = [root]
        while q:
            # 每一层第一个节点放入结果
            ans.append(q[0].val)
            for _ in range(len(q)):
                x = q.pop(0)
                # 队列中每一层节点的顺序是从右到左
                if x.right:
                    q.append(x.right)
                if x.left:
                    q.append(x.left)
        return ans
```

DFS, 按照 根->右->左的方式遍历，每一层的第一个肯定是最右边的元素，每一层的第一个写入到结果集中。

```python
class Solution(object):
    def rightSideView(self, root):
        if not root:
            return []
        
        ans = []
        def dfs(root, depth):
            if not root:
                return
            # 根据结果中的元素个数判断这一层是不是已经写入了元素。
            if len(ans) == depth:
                ans.append(root.val)
            
            dfs(root.right, depth+1)
            dfs(root.left, depth+1)
            
        dfs(root, 0)
        return ans
```


