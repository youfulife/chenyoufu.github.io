---
title: "拓扑排序 课程表系列"
date: 2021-09-09T22:26:14+08:00
draft: false
tags: ["算法", "BFS", "五星", "拓扑排序", "图"]
categories: ["技术"]
---

**题目**

* [207. 课程表](https://leetcode-cn.com/problems/course-schedule/)
* [210. 课程表 II](https://leetcode-cn.com/problems/course-schedule-ii/)
* [1136. 平行课程](https://leetcode-cn.com/problems/parallel-courses/)
* [269. 火星词典](https://leetcode-cn.com/problems/alien-dictionary/)

**解题思路**

这几个题目都是标准的拓扑排序的例题，只是返回的值不一样。

为什么「拓扑排序」的「广度优先遍历」没有使用 visited 数组。这是因为 

1. 「拓扑排序」需要在「有向无环图」的前提下才能得到拓扑排序的结果；
2. 而 visited 数组的作用正是因为图中有环，才需要使用它记住已经遍历过的顶点，但是「有向无环图」恰好是没有环的，因此无需使用 visited 数组；
3. 入度数组恰好起到了 visited 数组的作用，如果在遍历完成以后，还存在入度不为 00 的顶点，则说明图中存在环，不能到拓扑序。


### 207 课程表

你这个学期必须选修 numCourses 门课程，记为 0 到 numCourses - 1 。

在选修某些课程之前需要一些先修课程。 先修课程按数组 prerequisites 给出，其中 prerequisites[i] = [ai, bi] ，表示如果要学习课程 ai 则 必须 先学习课程  bi 。

例如，先修课程对 [0, 1] 表示：想要学习课程 0 ，你需要先完成课程 1 。

请你判断是否可能完成所有课程的学习？如果可以，返回 true ；否则，返回 false 。

**解题思路**

这个答案只需要返回 能否完成所有课程，所以不需要保存课程的信息，可以简化bfs的流程，不需要分层。

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


### 210 课程表 II

现在你总共有 n 门课需要选，记为 0 到 n-1。

在选修某些课程之前需要一些先修课程。 例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示他们: [0,1]

给定课程总量以及它们的先决条件，返回你为了学完所有课程所安排的学习顺序。

可能会有多个正确的顺序，你只要返回一种就可以了。如果不可能完成所有课程，返回一个空数组。

**解题思路**

这个答案需要返回课程的完成顺序，所以需要在bfs的过程中保存节点信息。

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

### 1136. 平行课程

已知有 N 门课程，它们以 1 到 N 进行编号。

给你一份课程关系表 relations[i] = [X, Y]，用以表示课程 X 和课程 Y 之间的先修关系：课程 X 必须在课程 Y 之前修完。

假设在一个学期里，你可以学习任何数量的课程，但前提是你已经学习了将要学习的这些课程的所有先修课程。

请你返回学完全部课程所需的最少学期数。

如果没有办法做到学完全部这些课程的话，就返回 -1。

**解题思路**

这个答案需要返回最短路径，所以需要在bfs中处理层级的逻辑。编号的是从1到N，所以下标相关的细节需要注意。

```python
class Solution(object):
    def minimumSemesters(self, n, relations):      
        v = [[] for _ in range(n+1)]
        d = [0] * (n+1)
        for x, y in relations:
            v[x].append(y)
            d[y] += 1
        
        step = 0
        q = deque([i for i in range(1, n+1) if d[i] == 0])
        while q:
            for _ in range(len(q)):
                i = q.popleft()
                for j in v[i]:
                    d[j] -= 1
                    if d[j] == 0:
                        q.append(j)
            step += 1
        return step if sum(d) == 0 else -1
```

#### 269. 火星词典

现有一种使用英语字母的火星语言，这门语言的字母顺序与英语顺序不同。

给你一个字符串列表 words ，作为这门语言的词典，words 中的字符串已经 按这门新语言的字母顺序进行了排序 。

请你根据该词典还原出此语言中已知的字母顺序，并 按字母递增顺序 排列。若不存在合法字母顺序，返回 "" 。若存在多种可能的合法字母顺序，返回其中 任意一种 顺序即可。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/alien-dictionary
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```python
class Solution(object):
    def alienOrder(self, words):
        """
        :type words: List[str]
        :rtype: str
        """
        g = {}
        indegree = [0] * 26

        for word in words:
            for c in word:
                g[c] = g.get(c, [])
        
        for i in range(len(words)-1):
            x, y = words[i], words[i+1]
            minlen = min(len(x), len(y))
            # ["abc","ab"] 不合法顺序
            if len(x) > len(y) and x[:len(y)] == y:
                return ""
            for j in range(minlen):
                if x[j] == y[j]:
                    continue
                
                if y[j] not in g[x[j]]:
                    g[x[j]].append(y[j])
                    indegree[ord(y[j]) - ord('a')] += 1

                break
        
        # print(g, indegree)
        ans = []
        q = [i for i in range(26) if indegree[i] == 0 and chr(ord('a') + i) in g]
        for i in q:
            c = chr(ord('a') + i)
            ans.append(c)
            for v in g[c]:
                j = ord(v) - ord('a')
                indegree[j] -= 1
                if indegree[j] == 0:
                    q.append(j)
        
        if len(ans) < len(g):
            return ""
        
        return ''.join(ans)
```