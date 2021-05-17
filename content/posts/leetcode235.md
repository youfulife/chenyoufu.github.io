---
title: "235 二叉搜索树的最近公共祖先"
date: 2021-05-16T08:52:06+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

**示例 1:**

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 8
输出: 6 
解释: 节点 2 和节点 8 的最近公共祖先是 6。
```

**示例 2:**

```
输入: root = [6,2,8,0,4,7,9,null,null,3,5], p = 2, q = 4
输出: 2
解释: 节点 2 和节点 4 的最近公共祖先是 2, 因为根据定义最近公共祖先节点可以为节点本身。
```

**说明:**

* 所有节点的值都是唯一的。
* p、q 为不同节点且均存在于给定的二叉搜索树中。


链接：https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-search-tree

**解题思路**

* 我们从根节点开始遍历；
* 如果当前节点的值大于 pp 和 qq 的值，说明 pp 和 qq 应该在当前节点的左子树，因此将当前节点移动到它的左子节点；
* 如果当前节点的值小于 pp 和 qq 的值，说明 pp 和 qq 应该在当前节点的右子树，因此将当前节点移动到它的右子节点；
* 如果当前节点的值不满足上述两条要求，那么说明当前节点就是「分岔点」。此时，pp 和 qq 要么在当前节点的不同的子树中，要么其中一个就是当前节点。

```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        cur = root
        while cur:
            if cur.val > p.val and cur.val > q.val:
                cur = cur.left
                continue
            if cur.val < p.val and cur.val < q.val:
                cur = cur.right
                continue
            # 分岔点
            break
        return cur
```

-----

进阶 236 二叉树的最近公共祖先