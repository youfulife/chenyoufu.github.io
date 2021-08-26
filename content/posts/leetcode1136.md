---
title: "1136 平行课程"
date: 2021-08-24T23:17:19+08:00
draft: false
tags: ["算法", "图", "拓扑排序", "BFS"]
categories: ["技术"]
---

**题目**

已知有 N 门课程，它们以 1 到 N 进行编号。

给你一份课程关系表 relations[i] = [X, Y]，用以表示课程 X 和课程 Y 之间的先修关系：课程 X 必须在课程 Y 之前修完。

假设在一个学期里，你可以学习任何数量的课程，但前提是你已经学习了将要学习的这些课程的所有先修课程。

请你返回学完全部课程所需的最少学期数。

如果没有办法做到学完全部这些课程的话，就返回 -1。

```
输入：N = 3, relations = [[1,3],[2,3]]
输出：2
解释：
在第一个学期学习课程 1 和 2，在第二个学期学习课程 3。
```

链接：https://leetcode-cn.com/problems/parallel-courses

**解题思路**

```python
class Solution(object):
    def minimumSemesters(self, n, relations):
        p = {}
        degree = [0] * n
        for x, y in relations:
            p[x - 1] = p.get(x - 1, []) + [y - 1]
            degree[y - 1] += 1 # y - 1 的度 +1，不是 x - 1
        q = deque([])
        for i in range(n):
            if degree[i] == 0:
                q.append(i)
        
        step = 0
        while q:
            for _ in range(len(q)):
                # 要popleft
                i = q.popleft()
                # p[j] 可能不存在，所以用p.get
                for j in p.get(i, []):
                    degree[j] -= 1
                    if degree[j] == 0:
                        q.append(j)
            step += 1
        for i in range(n):
            if degree[i] > 0:
                return -1
        return step
```