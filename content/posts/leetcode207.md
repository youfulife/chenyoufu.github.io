---
title: "207 课程表"
date: 2021-08-25T22:02:05+08:00
draft: false
tags: ["算法", "图", "拓扑排序", "BFS"]
categories: ["技术"]
---

**题目**

你这个学期必须选修 numCourses 门课程，记为 0 到 numCourses - 1 。

在选修某些课程之前需要一些先修课程。 先修课程按数组 prerequisites 给出，其中 prerequisites[i] = [ai, bi] ，表示如果要学习课程 ai 则 必须 先学习课程  bi 。

例如，先修课程对 [0, 1] 表示：想要学习课程 0 ，你需要先完成课程 1 。
请你判断是否可能完成所有课程的学习？如果可以，返回 true ；否则，返回 false 。

链接：https://leetcode-cn.com/problems/course-schedule

**解题思路**

```python
class Solution(object):
    def canFinish(self, numCourses, prerequisites):
        n = numCourses
        v = [[] for i in range(n)]
        degree = [0] * n
        # 初始化图，邻接表，入度数组
        for x, y in prerequisites:
            v[y].append(x)
            degree[x] += 1
        # 初始化队列，入度为0加入队列
        q = []
        for i in range(n):
            if degree[i] == 0:
                q.append(i)
        # BFS
        for i in q:
            for j in v[i]:
                degree[j] -= 1
                if degree[j] == 0:
                    q.append(j)
        return sum(degree) == 0 
```