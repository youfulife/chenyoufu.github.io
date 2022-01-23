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

* 若一个节点没有出边（走到了死胡同），则该节点是安全的；同时只指向这个节点的上游节点也是安全的。
* 拓扑排序考虑的是入度为0的节点加入队列，然后BFS。这里考虑的是出度为0的节点加入对列，然后BFS。
* 如何查找出度为0的节点的上游节点呢，可以将图中所有边反向，得到一个反图，然后在反图上运行拓扑排序。
* 最终拓扑排序完成后，出度为0的节点，就是安全节点。

```python
class Solution(object):
    def eventualSafeNodes(self, graph):
        n = len(graph)
        rg = [[] for _ in range(n)]
        out = [0] * n
        for i in range(n):
            for j in graph[i]:
                rg[j].append(i)
            out[i] = len(graph[i])
        
        q = [i for i in range(n) if out[i] == 0]
        for i in q:
            for j in rg[i]:
                out[j] -= 1
                if out[j] == 0:
                    q.append(j)
        return [i for i in range(n) if out[i] == 0]
```

**深度优先搜索 + 三色标记法**

我们可以使用深度优先搜索来找环，并在深度优先搜索时，用三种颜色对节点进行标记，标记的规则如下：

* 白色（用 00 表示）：该节点尚未被访问；
* 灰色（用 11 表示）：该节点位于递归栈中，或者在某个环上；
* 黑色（用 22 表示）：该节点搜索完毕，是一个安全节点。

当我们首次访问一个节点时，将其标记为灰色，并继续搜索与其相连的节点。

如果在搜索过程中遇到了一个灰色节点，则说明找到了一个环，此时退出搜索，栈中的节点仍保持为灰色，这一做法可以将「找到了环」这一信息传递到栈中的所有节点上。

如果搜索过程中没有遇到灰色节点，则说明没有遇到环，那么递归返回前，我们将其标记由灰色改为黑色，即表示它是一个安全的节点。

作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/find-eventual-safe-states/solution/zhao-dao-zui-zhong-de-an-quan-zhuang-tai-yzfz/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

```python
class Solution(object):
    def eventualSafeNodes(self, graph):
        n = len(graph)
        color = [0] * n # 0 not visited, 1 in stack, 2 safe

        def safe(x):
            if color[x] > 0:
                return color[x] == 2
            
            color[x] = 1
            for y in graph[x]:
                if not safe(y):
                    return False
            color[x] = 2
            return True
        
        return [x for x in range(n) if safe(x)]
```