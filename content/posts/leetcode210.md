---
title: "210 课程表 II"
date: 2021-08-25T22:06:31+08:00
draft: false
tags: ["算法", "图", "拓扑排序", "BFS"]
categories: ["技术"]
---

**题目**

现在你总共有 n 门课需要选，记为 0 到 n-1。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: [0,1]

给定课程总量以及它们的先决条件，返回你为了学完所有课程所安排的学习顺序。

可能会有多个正确的顺序，你只要返回一种就可以了。如果不可能完成所有课程，返回一个空数组。

链接：https://leetcode-cn.com/problems/course-schedule-ii

**解题思路**


```python
class Solution(object):
    def findOrder(self, numCourses, prerequisites):
        n = numCourses
        v = [[] for i in range(n)]
        d = [0] * n
        for x, y in prerequisites:
            v[y].append(x)
            d[x] += 1
        
        q = [i for i in range(n) if d[i] == 0]
        ans = []
        for i in q:
            ans.append(i)
            for j in v[i]:
                d[j] -= 1
                if d[j] == 0:
                    q.append(j)
        return ans if sum(d) == 0 else []
```

为什么「拓扑排序」的「广度优先遍历」没有使用 visited 数组。这是因为 
1. 「拓扑排序」需要在「有向无环图」的前提下才能得到拓扑排序的结果
2. 而 visited 数组的作用正是因为图中有环，才需要使用它记住已经遍历过的顶点，但是「有向无环图」恰好是没有环的，因此无需使用 visited 数组；
3. 入度数组恰好起到了 visited 数组的作用，如果在遍历完成以后，还存在入度不为 00 的顶点，则说明图中存在环，不能到拓扑序。

作者：力扣 (LeetCode)
链接：https://leetcode-cn.com/leetbook/read/bfs/ek9sl2/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。