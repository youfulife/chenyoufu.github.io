---
title: "684 冗余连接"
date: 2021-08-01T10:42:12+08:00
draft: false
tags: ["算法", "并查集", "BFS", "DFS"]
categories: ["技术"]
---

**题目**

树可以看成是一个连通且 无环 的 无向 图。

给定往一棵 n 个节点 (节点值 1～n) 的树中添加一条边后的图。添加的边的两个顶点包含在 1 到 n 中间，且这条附加的边不属于树中已存在的边。图的信息记录于长度为 n 的二维数组 edges ，edges[i] = [ai, bi] 表示图中在 ai 和 bi 之间存在一条边。

请找出一条可以删去的边，删除后可使得剩余部分是一个有着 n 个节点的树。如果有多个答案，则返回数组 edges 中最后出现的边。

链接：https://leetcode-cn.com/problems/redundant-connection

**解题思路**

并查集

```python
class Solution(object):

    class UFS(object):
        def __init__(self, n):
            self.parent = [i for i in range(n)]
            self.n = n

        def find(self, p):
            if p == self.parent[p]:
                return p
            self.parent[p] = self.find(self.parent[p])
            return self.parent[p]
        
        def union(self, p, q):
            self.parent[self.find(p)] = self.find(q)
            return
        
        def isConnected(self, p, q):
            return self.find(p) == self.find(q)


    def findRedundantConnection(self, edges):
        n = len(edges)
        ufs = Solution.UFS(n)
        for edge in edges:
            # 这里要注意，并查集的下标从0开始，题目给出的节点是从1开始
            p = edge[0] - 1
            q = edge[1] - 1
            if not ufs.isConnected(p, q):
                ufs.union(p, q)
            else:
                return edge
        return
```

------

#### 2021.09.07 DFS专题 再刷

**并查集**

```python
class Solution(object):
    class UFS():
        def __init__(self, n):
            self.count = n
            self.p = [i for i in range(n)]
        
        def find(self, x):
            if self.p[x] != x:
                self.p[x] = self.find(self.p[x])
            return self.p[x]
        
        def union(self, x, y):
            rootx = self.find(x)
            rooty = self.find(y)
            if rootx == rooty:
                return
            self.p[rootx] = rooty
            self.count -= 1
        
        def isConnected(self, x, y):
            return self.find(x) == self.find(y)

    def findRedundantConnection(self, edges):
        n = len(edges)
        ufs = Solution.UFS(n)
        ans = []
        for x, y in edges:
            if ufs.isConnected(x-1, y-1):
                ans.append([x, y])
            else:
                ufs.union(x-1, y-1)
        return ans[-1]
```

**DFS**

如果两个顶点其中有一个不在这张图上，它当然不是多余的边；

可以在添加一条边的时候，检查从其中一个顶点是否可以通过 遍历 到达另一个顶点，这里所说的遍历可以是「深度优先遍历」，也可以是「广度优先遍历」；

* 如果遍历不能到达，说明这条表不是多余的边，需要添加到图中（注意：无向图需要添加两条边）；
* 如果遍历可以到达，说明形成了回路，当前考虑的这条边就是多余的边。


```python
class Solution(object):
    def findRedundantConnection(self, edges):

        def isConnected(v, x, y, visited):
            if x == y:
                return True
            
            visited[x] = 1
            
            for i in v[x]:
                if visited[i] == 0:
                    if isConnected(v, i, y, visited):
                        return True
            return False

        n = len(edges)
        v = [[] for _ in range(n+1)]
        
        for x, y in edges:
            visited = [0] * (n+1)
            if isConnected(v, x, y, visited):
                return [x, y]
            else:
                v[x].append(y)
                v[y].append(x)
        return
```

**BFS**

