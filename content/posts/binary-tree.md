---
title: "二叉树系列"
date: 2022-06-25T10:56:59+08:00
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

* [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)
* [144. 二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/)
* [145. 二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/)
* [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)
* [105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
* [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
* [297. 二叉树的序列化与反序列化](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/)
* [449. 序列化和反序列化二叉搜索树](https://leetcode.cn/problems/serialize-and-deserialize-bst/)
* [1008. 前序遍历构造二叉搜索树](https://leetcode.cn/problems/construct-binary-search-tree-from-preorder-traversal/)
* [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)

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


### 98. 验证二叉搜索树

给你一个二叉树的根节点 root ，判断其是否是一个有效的二叉搜索树。

思路一，边界法

除了跟父节点对比，还要跟祖先对比

```python
class Solution(object):
    def isValidBST(self, root):
        def helper(root, low, high):

            if not root:
                return True
            
            if root.val <= low or root.val >= high:
                return  False
            
            return helper(root.left, low, root.val) and helper(root.right, root.val, high)
        
        return helper(root, float('-inf'), float('+inf'))
```

思路二，中序遍历

记录上一个节点的，在中序的过程中判断是不是严格递增，不需要占用一个空间先把遍历结果存下来然后在根据数组的值判断。

```python
class Solution(object):
    pre = None
    def isValidBST(self, root):
        if not root:
            return True
        
        if not self.isValidBST(root.left):
            return False
        
        if self.pre and self.pre.val >= root.val:
            return False
        self.pre = root

        if not self.isValidBST(root.right):
            return False
        
        return True
```




## 扩展应用

* 树的各种题型，深度，路径和，对称，反转，公共祖先，右视图

### 经典题目

* [199. 二叉树的右视图](https://leetcode-cn.com/problems/binary-tree-right-side-view/)
* [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)
* [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)
* [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)
* [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)
* [617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)
* [114. 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)
* [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)
* [437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/)

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

### 101. 对称二叉树

给你一个二叉树的根节点 root ， 检查它是否轴对称。

思路一，递归

```python
class Solution(object):
    def isSymmetric(self, root):
        if not root:
            return True
        
        def hepler(left, right):
            if not left or not right:
                return left == right
            
            return left.val == right.val and hepler(left.right, right.left) and hepler(left.left, right.right)
        
        return hepler(root.left, root.right)
```

思路二，迭代

```python
class Solution(object):
    def isSymmetric(self, root):
        q = [(root, root)]
        for x, y in q:
            if not x and not y:
                continue

            if not x or not y:
                return False
                
            if x.val != y.val:
                return False
            
            q.append((x.left, y.right))
            q.append((x.right, y.left))
        return True
```

### 104. 二叉树的最大深度

给定一个二叉树，找出其最大深度。二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

巧妙的递归

```python
class Solution(object):
    def maxDepth(self, root):
        if not root:
            return 0
        
        return 1 + max(self.maxDepth(root.left), self.maxDepth(root.right))
```

BFS

```python
class Solution(object):
    def maxDepth(self, root):
        if not root:
            return 0
        q = deque([root])
        depth = 0
        while q:
            for _ in range(len(q)):
                node = q.popleft()
                if node.left:
                    q.append(node.left)
                if node.right:
                    q.append(node.right)
            depth += 1
        return depth
```

DFS

```python
class Solution(object):
    def maxDepth(self, root):
        if not root:
            return 0
        stack = [(root, 1)]
        ret = 0
        while stack:
            node, depth = stack.pop()
            ret = max(depth, ret)
            if node.left:
                stack.append((node.left, depth+1))
            if node.right:
                stack.append((node.right, depth+1))

        return ret
```


### 226. 翻转二叉树

给你一棵二叉树的根节点 root ，翻转这棵二叉树，并返回其根节点。

自顶向下

```python
class Solution(object):
    def invertTree(self, root):
        if not root:
            return None
        
        root.left, root.right = root.right, root.left
        self.invertTree(root.left)
        self.invertTree(root.right)
        return root
```

自底向上

```python
class Solution(object):
    def invertTree(self, root):
        if not root:
            return root
        root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)
        return root
```

迭代，队列

```python
class Solution(object):
    def invertTree(self, root):
        if not root:
            return None
        
        q = [root]
        for x in q:
            x.left, x.right = x.right, x.left
            if x.left:
                q.append(x.left)
            if x.right:
                q.append(x.right)
        return root
```

### 543. 二叉树的直径

给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过也可能不穿过根结点。

```python
class Solution(object):
    def diameterOfBinaryTree(self, root):
        self.ans = 0
        def depth(root):
            if not root:
                return 0

            left = depth(root.left)
            right = depth(root.right)
            self.ans = max(self.ans, left + right)
            return 1 + max(left, right)
        depth(root)
        return self.ans
```

一篇文章解决所有二叉树路径问题（问题分析+分类模板+题目剖析）

https://leetcode-cn.com/problems/diameter-of-binary-tree/solution/yi-pian-wen-zhang-jie-jue-suo-you-er-cha-6g00/

### 617. 合并二叉树

你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，不为 null 的节点将直接作为新二叉树的节点。

```python
class Solution(object):
    def mergeTrees(self, root1, root2):
        """
        :type root1: TreeNode
        :type root2: TreeNode
        :rtype: TreeNode
        """
        if not root1 or not root2:
            return root2 or root1
        
        root = TreeNode(root1.val + root2.val)
        root.left = self.mergeTrees(root1.left, root2.left)
        root.right = self.mergeTrees(root1.right, root2.right)
        
        return root
```

### 114. 二叉树展开为链表

给你二叉树的根结点 root ，请你将它展开为一个单链表：

展开后的单链表应该同样使用 TreeNode ，其中 right 子指针指向链表中下一个结点，而左子指针始终为 null 。
展开后的单链表应该与二叉树 先序遍历 顺序相同。

思路一，递归

```python
class Solution(object):
    def flatten(self, root):
        """
        :type root: TreeNode
        :rtype: None Do not return anything, modify root in-place instead.
        """
        if not root:
            return None
        
        left = root.left
        right = root.right
        self.flatten(root.left)
        self.flatten(root.right)

        root.left = None
        root.right = left
        
        while root.right:
            root = root.right
        # root.left = None
        root.right = right
        
        return
```

思路二，迭代

```python
class Solution(object):
    def flatten(self, root):
        while root:
            if not root:
                return

            if not root.left:
                root = root.right
            else:
                p = root.left
                while p.right:
                    p = p.right
                p.right = root.right
                root.right = root.left
                root.left = None
                root = root.right
                
        return
```

### 236 二叉树的最近公共祖先


这个题目好难理解。


https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/solution/c-jing-dian-di-gui-si-lu-fei-chang-hao-li-jie-shi-/

https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/solution/236-er-cha-shu-de-zui-jin-gong-gong-zu-xian-hou-xu/


思路一， 根据p, q在root的两侧还是同一侧，进行判断，然后递归处理。

后序遍历，一层一层向上传递东西，在根结点汇总。

超时

```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        """
        :type root: TreeNode
        :type p: TreeNode
        :type q: TreeNode
        :rtype: TreeNode
        """
                
        if not root:
            return None

        def isDescendant(root, target):
            if not root:
                return False

            if root == target:
                return True
            
            return isDescendant(root.left, target) or isDescendant(root.right, target)

        if root == p or root == q:
            return root

        if isDescendant(root.left, p) and isDescendant(root.right, q):
            return root
        
        if isDescendant(root.left, q) and isDescendant(root.right, p):
            return root
        
        if isDescendant(root.left, p) and isDescendant(root.left, q):
            return self.lowestCommonAncestor(root.left, p, q)
        
        if isDescendant(root.right, p) and isDescendant(root.right, q):
            return self.lowestCommonAncestor(root.right, p, q)
```

通过

```python
class Solution(object):
    def lowestCommonAncestor(self, root, p, q):
        if not root:
            return None
        if root == p or root == q:
            return root
        
        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left and right:
            return root
        if left:
            return left
        if right:
            return right
        
        return None
```

### 437. 路径总和 III

给定一个二叉树的根节点 root ，和一个整数 targetSum ，求该二叉树里节点值之和等于 targetSum 的 路径 的数目。

```python
class Solution(object):
    def pathSum(self, root, targetSum):
        if not root:
            return 0

        def dfs(root, target):
            if not root:
                return 0
            ans = 0
            if target == root.val:
                ans += 1
                
            ans += dfs(root.left, target-root.val)
            ans += dfs(root.right, target-root.val)
            return ans
        
        def helper(root, target):
            if not root:
                return 0
            ans = dfs(root, target)
            ans += helper(root.left, target)
            ans += helper(root.right, target)
            return ans
        
        return helper(root, targetSum)
```