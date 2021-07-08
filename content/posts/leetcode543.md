---
title: "543 二叉树的直径"
date: 2021-07-07T22:57:36+08:00
draft: false
tags: ["算法", "树", "多刷", "hot100"]
categories: ["技术"]
---

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

示例 :
给定二叉树
```

          1
         / \
        2   3
       / \     
      4   5    
```
返回 3, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

注意：两结点之间的路径长度是以它们之间边的数目表示。

链接：https://leetcode-cn.com/problems/diameter-of-binary-tree

**解题思路**

* 直径？第一次听说这个概念
* 感觉跟高度有关系
* 任意节点

深度优先搜索

首先我们知道一条路径的长度为该路径经过的节点数减一，所以求直径（即求路径长度的最大值）等效于求路径经过节点数的最大值减一。

而任意一条路径均可以被看作由某个节点为起点，从其左儿子和右儿子向下遍历的路径拼接得到。

```python
class Solution(object):
    def diameterOfBinaryTree(self, root):
        self.ans = 0
        def depth(root):
            if not root:
                return 0
            left = depth(root.left)
            right = depth(root.right)
            self.ans = max(self.ans, left + right+1)
            return 1 + max(left, right)
        depth(root)
        return self.ans - 1
```

相关题目

104. 二叉树的最大深度 （基本题型）

1372. 二叉树中的最长交错路径（周赛）

1367. 二叉树中的列表（周赛）

一篇文章解决所有二叉树路径问题（问题分析+分类模板+题目剖析）

https://leetcode-cn.com/problems/diameter-of-binary-tree/solution/yi-pian-wen-zhang-jie-jue-suo-you-er-cha-6g00/