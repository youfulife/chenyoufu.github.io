---
title: "94 二叉树的中序遍历"
date: 2021-06-13T22:16:11+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

**非递归**

```python
class Solution(object):
    def inorderTraversal(self, root):
        if not root:
            return []
        
        ans = []
        stack = []
        cur = root
        while cur or stack:
            while cur:
                stack.append(cur)
                cur = cur.left
            # 最左侧的节点，可能是叶子节点也可能是非叶子节点
            node = stack.pop() 
            ans.append(node.val)
            cur = node.right # 叶子结点，cur为空，下一次while直接执行pop，返回的是根节点。
        return ans
```
