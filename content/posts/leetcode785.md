---
title: "785 判断二分图"
date: 2022-01-26T22:03:58+08:00
draft: false
tags: ["算法", "并查集", "BFS", "DFS", "五星"]
categories: ["技术"]
---

**题目**

给定一个无向图graph，当这个图为二分图时返回true。

如果我们能将一个图的节点集合分割成两个独立的子集A和B，并使图中的每一条边的两个节点一个来自A集合，一个来自B集合，我们就将这个图称为二分图。

graph将会以邻接表方式给出，graph[i]表示图中与节点i相连的所有节点。每个节点都是一个在0到graph.length-1之间的整数。这图中没有自环和平行边： graph[i] 中不存在i，并且graph[i]中没有重复的值。

链接：https://leetcode-cn.com/problems/is-graph-bipartite/

**解题思路**

对于图中的任意两个节点 uu 和 vv，如果它们之间有一条边直接相连，那么 uu 和 vv 必须属于不同的集合。

如果给定的无向图连通，那么我们就可以任选一个节点开始，给它染成红色。随后我们对整个图进行遍历，将该节点直接相连的所有节点染成绿色，表示这些节点不能与起始节点属于同一个集合。我们再将这些绿色节点直接相连的所有节点染成红色，以此类推，直到无向图中的每个节点均被染色。

如果我们能够成功染色，那么红色和绿色的节点各属于一个集合，这个无向图就是一个二分图；如果我们未能成功染色，即在染色的过程中，某一时刻访问到了一个已经染色的节点，并且它的颜色与我们将要给它染上的颜色不相同，也就说明这个无向图不是一个二分图。


```python
class Solution(object):
    def isBipartite(self, graph):
        """
        :type graph: List[List[int]]
        :rtype: bool
        输入：graph = [[1,2,3],[0,2],[0,1,3],[0,2]]
        输出：false
        """
        n = len(graph)
        color = [0] * n

        visited = [0] * n

        def dfs(x, c):

            visited[x] = 1
            color[x] = c

            for i in graph[x]:
                if visited[i] == 1:
                    if color[i] == c:
                        return False
                else:
                    if not dfs(i, 1-c):
                        return False

            return True

        for i in range(n):
            if visited[i] == 0:
                if not dfs(i, 0):
                    return False

        return True
```


扩展：

什么是二分图呢？用百度百科的解释就是：二分图的顶点集可分割为两个互不相交的子集，图中每条边依附的两个顶点都分属于这两个子集，且两个子集内的顶点不相邻。

比如说：给你一幅「图」，请你用两种颜色将图中的所有顶点着色，且使得任意一条边的两个端点的颜色都不相同，你能做到吗？

这就是图的「双色问题」，其实这个问题就等同于二分图的判定问题，如果你能够成功地将图染色，那么这幅图就是一幅二分图，反之则不是：

二分图结构在某些场景可以更高效地存储数据，举例来说，如何存储电影演员和电影之间的关系？如果用哈希表存储，需要两个哈希表分别存储「每个演员到电影列表」的映射和「每部电影到演员列表」的映射。但如果用「图」结构存储，将电影和参演的演员连接，很自然地就成为了一幅二分图

每个电影节点的相邻节点就是参演该电影的所有演员，每个演员的相邻节点就是该演员参演过的所有电影
在某些场景下图结构也可以作为存储键值对的数据结构（符号表）
所以题目的思路很简单，就是遍历一遍图，一边遍历一边染色，看看能不能用两种颜色给所有节点染色，且相邻节点的颜色都不相同

作者：angela-x
链接：https://leetcode-cn.com/problems/is-graph-bipartite/solution/xi-shuo-er-fen-tu-by-angela-x-mgxc/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```python
class Solution(object):
    def isBipartite(self, graph):
        """
        :type graph: List[List[int]]
        :rtype: bool
        """
        n = len(graph)
        visited = [-1] * n # -1, 0, 1

        def dfs(i, c):
            visited[i] = c
            for j in graph[i]:
                if visited[j] == -1:
                    if not dfs(j, 1-c):
                        return False
                else:
                    if visited[j] == c:
                        return False
            return True
            

        for i in range(n):
            if visited[i] == -1:
                if not dfs(i, 0):
                    return False
        
        return True
```