---
title: "743 网络延迟时间"
date: 2022-01-23T18:09:14+08:00
draft: false
tags: ["算法", "图", "Dijstra", "最短路径"]
categories: ["技术"]
---

**题目**

有 n 个网络节点，标记为 1 到 n。

给你一个列表 times，表示信号经过 有向 边的传递时间。 times[i] = (ui, vi, wi)，其中 ui 是源节点，vi 是目标节点， wi 是一个信号从源节点传递到目标节点的时间。

现在，从某个节点 K 发出一个信号。需要多久才能使所有节点都收到信号？如果不能使所有节点收到信号，返回 -1 。


链接：https://leetcode-cn.com/problems/network-delay-time

**解题思路**

狄杰斯特拉算法，具体算法介绍参考：https://www.cnblogs.com/goldsunshine/p/12978305.html

```python
class Solution(object):
    def networkDelayTime(self, times, n, k):
        """
        :type times: List[List[int]]
        :type n: int
        :type k: int
        :rtype: int
        """
        # weight 矩阵
        # dist 最短路径
        # visited 已经确定的最短路径的点
        # 初始化 距离 为MAX
        # 利用贪心思想，更新dist距离
        
        MAX = float('+inf')

        w = [[MAX] * n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                if i == j:
                    w[i][j] = 0
        for u, v, d in times:
            w[u-1][v-1] = d

        used = [False] * n

        dist = [MAX] * n
        dist[k-1] = 0

        for _ in range(n):

            x = -1
            for i in range(n):
                if not used[i] and (x == -1 or dist[i] < dist[x]):
                    x = i
            
            used[x] = True

            for i in range(n):
                # 易错点，一开始写成了 w[k-1][x] + w[x][i]
                dist[i] = min(dist[i], dist[x] + w[x][i]) 
        
        # 易错点，ans等于最大值的时候需要返回 -1
        ans = max(dist)
        return ans if  ans < MAX else -1
```

