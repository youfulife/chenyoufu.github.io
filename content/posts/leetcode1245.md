---
title: "1245 树的直径"
date: 2021-08-26T23:07:06+08:00
draft: false
tags: ["算法", "图", "拓扑排序", "BFS", "五星"]
categories: ["技术"]
---

**题目**

* 逐渐将每一层最外层节点（入度为1的节点）移除，直到最后剩下小于2个节点结束
* level用来记录层数，最后结果就是level*2或者level*2+1

```python
class Solution(object):
    def treeDiameter(self, edges):
        n = len(edges) + 1

        v = [[] for i in range(n)]
        degree = [0] * n
        for x, y in edges:
            v[x].append(y)
            v[y].append(x)
            degree[x] += 1
            degree[y] += 1
        
        q = deque([])
        for i in range(n):
            if degree[i] == 1:
                q.append(i)
                degree[i] = 0
        step = 0
        while n > 2:
            qsize = len(q)
            n -= qsize
            for _ in range(qsize):
                i = q.popleft()
                for j in v[i]:
                    degree[j] -= 1
                    if degree[j] == 1:
                        q.append(j)
            
            step += 1
        ans = 0
        if n == 2:
            ans = 2 * step + 1
        else:
            ans = 2 * step
        
        return ans
```

**直径通用算法**

一般求解树的直径的问题，最简单高效的方法是先从任意一个点出发进行搜索，直到走到一个最远的点，我们可以证明这个点一定是直径的一个端点，然后再从这个点为起点进行搜索，直到到达从该点出发到达的最远的点，其经过的路径一定是树的直径。

* 第一次BFS：随机选择一个顶点开始，记录最长的距离的节点node
* 第二次BFS：从node开始BFS，能到达的最远距离即为所求

详细证明：

https://www.cnblogs.com/wuyiqi/archive/2012/04/08/2437424.html




**DFS**