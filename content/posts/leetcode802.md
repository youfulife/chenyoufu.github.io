---
title: "802 找到最终的安全状态"
date: 2021-08-28T15:52:00+08:00
draft: false
tags: ["算法", "图", "拓扑排序", "BFS", "DFS", "五星", "逆向思维"]
categories: ["技术"]
---

**题目**

在有向图中，以某个节点为起始节点，从该点出发，每一步沿着图中的一条有向边行走。如果到达的节点是终点（即它没有连出的有向边），则停止。

对于一个起始节点，如果从该节点出发，无论每一步选择沿哪条有向边行走，最后必然在有限步内到达终点，则将该起始节点称作是 安全 的。

返回一个由图中所有安全的起始节点组成的数组作为答案。答案数组中的元素应当按 升序 排列。

该有向图有 n 个节点，按 0 到 n - 1 编号，其中 n 是 graph 的节点数。图以下述形式给出：graph[i] 是编号 j 节点的一个列表，满足 (i, j) 是图的一条有向边。

```
输入：graph = [[1,2],[2,3],[5],[0],[5],[],[]]
输出：[2,4,5,6]
```

链接：https://leetcode-cn.com/problems/find-eventual-safe-states

**解题思路**

**拓扑排序**

* 若一个节点没有出边，则该节点是安全的；若一个节点出边相连的点都是安全的，则该节点也是安全的。
* 将图中所有边反向，得到一个反图，然后在反图上运行拓扑排序。

```python
class Solution(object):
    def eventualSafeNodes(self, graph):
        n = len(graph)
        ins = [[] for _ in range (n)] # 反向
        q = []
        for i in range(n):
            for v in graph[i]:
                ins[v].append(i)
            if len(graph[i]) == 0:
                q.append(i)
        
        ans = []
        for i in q:
            ans.append(i)
            for j in ins[i]:
                graph[j].remove(i)
                if len(graph[j]) == 0:
                    q.append(j)
        return sorted(ans)
```

**深度优先搜索 + 三色标记法**

