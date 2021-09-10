---
title: "886 可能的二分法"
date: 2021-09-09T23:26:14+08:00
draft: false
tags: ["算法", "DFS", "BFS", "并查集", "二分图", "图", "五星"]
categories: ["技术"]
---

**题目**

给定一组 N 人（编号为 1, 2, ..., N）， 我们想把每个人分进任意大小的两组。

每个人都可能不喜欢其他人，那么他们不应该属于同一组。

形式上，如果 dislikes[i] = [a, b]，表示不允许将编号为 a 和 b 的人归入同一组。

当可以用这种方法将所有人分进两组时，返回 true；否则返回 false。

示例 1：

输入：N = 4, dislikes = [[1,2],[1,3],[2,4]]
输出：true
解释：group1 [1,4], group2 [2,3]
示例 2：

输入：N = 3, dislikes = [[1,2],[1,3],[2,3]]
输出：false

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/possible-bipartition
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**解题思路**

这道题是典型的图着色问题。

可以把 dislikes 中每个元组看成结点之间的边，根据结点关系画出图，相邻的结点之间的颜色不能相同。如果我们能够利用两种颜色把所有结点着色就说明可以把这些结点分为两类。

DFS 染色，自己写，代码很烂，各种分支判断，好几次才通过。

```python
class Solution(object):
    def possibleBipartition(self, n, dislikes):
        def dfs(v, i, color, visited):
            for j in v[i]:
                if visited[j] == color:
                    return False
                elif visited[j] == 0:
                    visited[j] = 3 - color
                    if not dfs(v, j, 3 - color, visited):
                        return False
            return True

        v = [[] for i in range(n+1)]
        for x, y in dislikes:
            v[x].append(y)
            v[y].append(x)
        
        visited = [0] * (n+1)

        for i in range(1, n+1):
            if visited[i] == 0:
                if not dfs(v, i, 1, visited):
                    return False
        return True
```

BFS 代码会清晰一些

```python
class Solution(object):
    def possibleBipartition(self, n, dislikes):
        v = [[] for i in range(n+1)]
        for x, y in dislikes:
            v[x].append(y)
            v[y].append(x)
        
        color = [0] * (n+1)
        # 因为图中可能含有多个连通域，所以我们需要判断是否存在顶点未被访问，若存在则从它开始再进行一轮 bfs 染色。
        for i in range(1, n):
            if color[i] != 0:
                continue
            q = [i]
            color[i] = 1
            for i in q:
                for j in v[i]:
                    if color[j] == color[i]:
                        return False
                    if color[j] == 0:
                        color[j] = -color[i]
                        q.append(j)
        return True
```

**并查集**

如果是二分图的话，那么图中每个顶点的所有邻接点都应该属于同一集合，且不与顶点处于同一集合。因此我们可以使用并查集来解决这个问题，我们遍历图中每个顶点，将当前顶点的所有邻接点进行合并，并判断这些邻接点中是否存在某一邻接点已经和当前顶点处于同一个集合中了，若是，则说明不是二分图。

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
            return
        
        def isConnected(self, x, y):
            return self.find(x) == self.find(y)

    def possibleBipartition(self, n, dislikes):
    
        v = [[] for i in range(n+1)]
        for x, y in dislikes:
            v[x].append(y)
            v[y].append(x)
        
        ufs = Solution.UFS(n+1)
        for i in range(1, n+1):
            if len(v[i]) > 0:
                for j in v[i]:
                    ufs.union(v[i][0], j)
                    if ufs.isConnected(i, j):
                        return False
        return True
```


参考：

https://leetcode-cn.com/problems/is-graph-bipartite/solution/bfs-dfs-bing-cha-ji-san-chong-fang-fa-pan-duan-er-/