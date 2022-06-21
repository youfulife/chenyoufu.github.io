---
title: "二叉树系列"
date: 2022-02-19T16:56:59+08:00
draft: false
tags: ["算法", "树"]
categories: ["技术"]
---

## 基础

* 二叉树的遍历，递归 + 非递归，前中后层
* 二叉树的构建，序列化
* 二叉搜索树
* 优先级队列，堆
* 最小生成树

### 经典题目

[94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)
[144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)
[145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/)
[102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)
[105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
[106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
[297. 二叉树的序列化与反序列化](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/)
[449. 序列化和反序列化二叉搜索树](https://leetcode.cn/problems/serialize-and-deserialize-bst/)
[1008. 前序遍历构造二叉搜索树](https://leetcode.cn/problems/construct-binary-search-tree-from-preorder-traversal/)

### 二叉树的遍历

给你二叉树的根节点 root ，返回它节点值的 前序，中序，后序 遍历。

**解题思路**

递归， 前中后代码一样，只是结果返回的位置变化一下。

```python
class Solution(object):
    def preorderTraversal(self, root):
        def dfs(root, ans):
            if not root:
                return

            # 前序
            ans.append(root.val)

            dfs(root.left, ans)

            # 中序
            # ans.append(root.val)

            dfs(root.right, ans)

            # 后序
            # ans.append(root.val)
            return
        ans = []
        dfs(root, ans)
        return ans
```

非递归

非递归有两种方法，一种是通常的方法，三种遍历代码不同，还有一种是基于颜色标记法，三种代码相同。

前序遍历，栈

```python
class Solution(object):
    def preorderTraversal(self, root):
        if not root:
            return []

        ans = []
        stack = [root]
        while stack:
            x = stack.pop()
            ans.append(x.val)
            if x.right:
                stack.append(x.right)
            if x.left:
                stack.append(x.left)
        return ans
```

中序遍历


后序遍历

思路

* 先序遍历顺序：根节点-左孩子-右孩子
* 后序遍历顺序：左孩子-右孩子-根节点
* 后序遍历倒过来：根节点-右孩子-左孩子

第一步，将二叉树按照先序非递归算法进行遍历，注意在入栈的时候左右孩子入栈的顺序，先左后右。

第二步，将遍历得到的结果进行倒置。 

这个思路实际上还是很巧妙的，值得学习。

```python
class Solution(object):
    def postorderTraversal(self, root):
        if not root:
           return []
        
        ans = []
        stack = [root]
        while stack:
            node = stack.pop()
            ans.append(node.val)
            if node.left:
                stack.append(node.left)
            if node.right:
                stack.append(node.right)
        return ans[::-1]
```


染色法，前中后序通用遍历模版。

https://leetcode-solution-leetcode-pp.gitbook.io/leetcode-solution/thinkings/binary-tree-traversal

用两种颜色，表示节点是不是需要被处理还是只是经过，不同的顺序只需要按照定义调整这三行的顺序即可，和递归的写法类似。

```python
stack.append((root.right, 0))
stack.append((root, 1))
stack.append((root.left, 0))
```

```python
class Solution(object):
    def postorderTraversal(self, root):
        """
        :type root: TreeNode
        :rtype: List[int]
        """
        if not root:
            return []
        red, green = 0, 1
        stack = [(root, red)]
        ans = []
        while stack:
            
            x, color = stack.pop()
            if color == red:
                # 后序
                stack.append((x, green))

                if x.right:
                    stack.append((x.right, red))

                # 中序
                # stack.append((x, green))

                if x.left:
                    stack.append((x.left, red))

                # 前序
                # stack.append((x, green))
            else:
                ans.append(x.val)
        return ans
```


### 102. 二叉树的层序遍历

给你二叉树的根节点 root ，返回其节点值的 层序遍历 。 （即逐层地，从左到右访问所有节点）。

**解题思路**

非递归



```python
class Solution(object):
    def levelOrder(self, root):
        if not root:
            return []
        q = collections.deque([root])
        ans = []
        while q:
            level = []
            for _ in range(len(q)):
                n = q.popleft()
                level.append(n.val)
                if n.left:
                    q.append(n.left)
                if n.right:
                    q.append(n.right)
            ans.append(level)
        return ans
```

递归

1. Use a variable to track level in the tree and use simple Pre-Order traversal
2. Add sub-lists to result as we move down the levels
3. Time Complexity: O(N)
4. Space Complexity: O(N) + O(h) for stack space

```python
class Solution(object):
    def levelOrder(self, root):
        if not root:
            return []
        def dfs(root, level, ans):
            if len(ans) > level:
                ans[level].append(root.val)
            else:
                ans.append([root.val])
            if root.left:
                dfs(root.left, level+1, ans)
            if root.right:
                dfs(root.right, level+1, ans)
        
        ans = []
        dfs(root, 0, ans)
        return ans
```


### 二叉树的构建

1. 前序和后序其实是一样的，前序 根左右，变成 根右左，倒序 左右根，就是后序，所以前序能做的后序都可以做。
2. 前序和后序是不能还原一颗二叉树的，比如，只有一个孩子的节点，无法判断是左孩子还是右孩子。

基于前序和中序

```python
class Solution(object):
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: TreeNode
        """
        if len(preorder) == 0:
            return None

        root = TreeNode(preorder[0])

        mid = 0
        for i in range(len(inorder)):
            if inorder[i] == preorder[0]:
                mid = i
                break
        root.left = self.buildTree(preorder[1:mid + 1], inorder[:mid])
        root.right = self.buildTree(preorder[mid + 1:], inorder[mid + 1:])

        return root
```

基于后序和中序

```python
def buildTree(self, inorder, postorder):
        if len(postorder) == 0:
            return None

        root = TreeNode(postorder[-1])

        mid = 0
        for i in range(len(inorder)):
            if inorder[i] == postorder[-1]:
                mid = i
                break

        root.left = self.buildTree(inorder[:mid], postorder[:mid])
        root.right = self.buildTree(inorder[mid+1:], postorder[mid:-1])
        return root
```


### 二叉树序列化

先序

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Codec:
    
    def serialize(self, root):
        """Encodes a tree to a single string.
        
        :type root: TreeNode
        :rtype: str
        """
        if not root:
            return '#'
        
        return str(root.val) + ',' + self.serialize(root.left) + ',' + self.serialize(root.right)

    def deserialize(self, data):
        """Decodes your encoded data to tree.
        
        :type data: str
        :rtype: TreeNode
        """
        def helper(data):
            # print(data)
            x = data.pop(0)
            if x == '#':
                return None
            root = TreeNode(int(x))
            root.left = helper(data)
            # print('---')
            root.right = helper(data)
            # print('ret')
            return root
        return helper(data.split(','))
```

层序

每一个非空节点都会对应两个子节点，那么反序列化的思路也是用队列进行层级遍历，同时用索引 i 记录对应子节点的位置

```python
class Codec:

    def serialize(self, root):
        ans = []
        q = [root]
        for x in q:
            if not x:
                ans.append('#')
            else:
                ans.append(str(x.val))
                q.append(x.left)
                q.append(x.right)
        return ','.join(ans)

    def deserialize(self, data):
        if len(data) == 0 or data[0] == '#':
            return None
        arr = data.split(',')
        root = TreeNode(int(arr[0]))
        q = [root]
        i = 0
        # x是root, i是root对应的下标, i+1是left, i+2是right
        for x in q:            
            if arr[i+1] != '#':
                x.left = TreeNode(int(arr[i+1]))
                q.append(x.left)
            
            if arr[i+2] != '#':
                x.right = TreeNode(int(arr[i+2]))
                q.append(x.right)
            i += 2
        return root
```


参考

https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/solution/zhongx-by-pedantic-elioniub-csrz/

https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/solution/shou-hui-tu-jie-gei-chu-dfshe-bfsliang-chong-jie-f/


https://www.cnblogs.com/labuladong/p/13975069.html


### 二叉搜索树构建

这个题目非常经典，有多种不同的解法。

思路一：根据二叉搜索树和前序遍历的特性，找到左子树和右子树，然后递归处理。

```python
def bstFromPreorder(self, preorder):
        """
        :type preorder: List[int]
        :rtype: TreeNode
        """
        if len(preorder) == 0:
            return None

        root = TreeNode(preorder[0])

        # 二分查找左右子树的边界，其实就是查找比target的数的左边界。
        target = preorder[0]
        left = 1
        right = len(preorder)-1
        ans = len(preorder)
        while left <= right:
            mid = int(left + (right-left)/2)
            if preorder[mid] < target:
                left = mid+1
            else:
                ans = mid
                right = mid-1
        right = ans
        # 线性查找
        # for i in range(1, len(preorder)):
        #     if preorder[i] > preorder[0]:
        #         right = i
        #         break
        root.left = self.bstFromPreorder(preorder[1:right])
        root.right = self.bstFromPreorder(preorder[right:])
        return root
```

思路二：

数组排序后正好就是二叉搜索树的中序遍历，然后结合前序遍历，转变为普通的根据前序+中序还原二叉树

思路三：

todo


### 二叉搜索树序列化


## 扩展应用

* 树的各种题型，深度，路径和，对称，反转，公共祖先，右视图

### 经典题目

[199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)


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
