---
title: "323 无向图中连通分量的数目"
date: 2021-08-08T23:05:50+08:00
draft: false
tags: ["算法", "并查集", "BFS"]
categories: ["技术"]
---
**题目**

给定编号从 0 到 n-1 的 n 个节点和一个无向边列表（每条边都是一对节点），请编写一个函数来计算无向图中连通分量的数目。

示例 1:
```
输入: n = 5 和 edges = [[0, 1], [1, 2], [3, 4]]

     0          3
     |          |
     1 --- 2    4 

输出: 2
```

链接：https://leetcode-cn.com/problems/number-of-connected-components-in-an-undirected-graph

**解题思路**

DFS，BFS需要建立邻接链表表示graph，然后在其基础上用栈（隐形）或者队列进行深度或宽度优先搜索。每次把能够连接到一起的边设置为true，如果已经访问过就跳过，不计数，最终unique的开始点的个数即为联通分量的个数。

Union Find 也类似，无需建立邻接链表，直接用edges的输入即可。跟朋友圈问题一样，把所有能连接到一起的节点都归结到一个root下，最后统计一下有多少个root即可。

**BFS**

目前是开始练习BFS系列，所以优先使用BFS来解决。

```python
class Solution(object):

    def bfs(self, adj, i, visited):
        q = deque([i])
        visited[i] = 1
        while q:
            p = q.popleft()
            for j in adj[p]:
                if visited[j] == 0:
                    q.append(j)
                    # 特别注意：在加入队列以后一定要将该结点标记为访问，否则会出现结果重复入队的情况
                    visited[j] = 1
        return

    def countComponents(self, n, edges):
        # 初始化图，邻接表
        adj = []
        for i in range(n):
            adj.append([])
        # 无向图，所以需要添加双向引用
        for edge in edges:
            adj[edge[0]].append(edge[1])
            adj[edge[1]].append(edge[0])
        # bfs遍历所有节点
        ans = 0
        visited = [0] * n
        for i in range(n):
            if visited[i] == 0:
                self.bfs(adj, i, visited)
                ans += 1
        return ans
```

**并查集**
