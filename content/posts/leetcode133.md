---
title: "133 克隆图"
date: 2022-01-15T23:26:47+08:00
draft: false
tags: ["算法", "图", "DFS", "BFS"]
categories: ["技术"]
---

**题目**

给你无向连通图中一个节点的引用，请你返回该图的深拷贝（克隆）。

图中的每个节点都包含它的值 val（int） 和其邻居的列表（list[Node]）。

```
class Node {
    public int val;
    public List<Node> neighbors;
}
```

链接：https://leetcode-cn.com/leetbook/read/dfs/eqi9cs/


**解题思路**

DFS

```python
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

class Solution(object):
    def __init__(self):
        self.visited = {}

    def cloneGraph(self, node):
        """
        :type node: Node
        :rtype: Node
        """
        if not node:
            return None

        if node in self.visited:
            return self.visited[node]
        
        cloneNode = Node(node.val, [])
        self.visited[node] =  cloneNode
        cloneNode.neighbors = [self.cloneGraph(n) for n in node.neighbors]
        
        return cloneNode
```

BFS

```python
"""
# Definition for a Node.
class Node(object):
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

class Solution(object):
    def __init__(self):
        self.visited = {}

    def cloneGraph(self, node):
        """
        :type node: Node
        :rtype: Node
        """
        if not node:
            return node

        q = [node]
        self.visited[node] = Node(node.val, [])

        for x in q:
            
            for y in x.neighbors:
                if y not in self.visited:
                    self.visited[y] = Node(y.val, [])
                    q.append(y)
                self.visited[x].neighbors.append(self.visited[y])

        
        return self.visited[node]
```