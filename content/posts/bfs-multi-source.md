---
title: "多源BFS"
date: 2022-02-05T21:58:49+08:00
draft: false
tags: ["算法", "BFS", "套路模版", "多源BFS"]
categories: ["技术"]
---

**题目**


**542. 01 矩阵**

给定一个由 0 和 1 组成的矩阵 mat ，请输出一个大小相同的矩阵，其中每一个格子是 mat 中对应位置元素到最近的 0 的距离。

两个相邻元素间的距离为 1 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/01-matrix
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**解题思路**

首先把每个源点 0 入队，然后从各个 0 同时开始一圈一圈的向 1 扩散（每个 1 都是被离它最近的 0 扩散到的 ），跟腐烂的橘子题目思路一样。

```python
class Solution(object):
    def updateMatrix(self, mat):
        """
        :type mat: List[List[int]]
        :rtype: List[List[int]]
        """
        m = len(mat)
        n = len(mat[0])
        visited = [[0] * n for _ in range(m)]
        q = collections.deque([])
        for i in range(m):
            for j in range(n):
                if mat[i][j] == 0:
                    q.append((i, j))
                    visited[i][j] = 1
        
        step = 0
        while q:
            for _ in range(len(q)):
                x, y = q.popleft()
                for dx, dy in [(-1, 0), (1, 0), (0, 1), (0, -1)]:
                    newx, newy = x + dx, y + dy
                    if 0<=newx<m and 0<=newy<n and visited[newx][newy] == 0:
                        mat[newx][newy] += step
                        visited[newx][newy] = 1
                        q.append([newx, newy])
            step += 1
        return mat
```

**994. 腐烂的橘子**

在给定的网格中，每个单元格可以有以下三个值之一：

* 值 0 代表空单元格；
* 值 1 代表新鲜橘子；
* 值 2 代表腐烂的橘子。
* 每分钟，任何与腐烂的橘子（在 4 个正方向上）相邻的新鲜橘子都会腐烂。

返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 -1。

链接：https://leetcode-cn.com/problems/rotting-oranges

**解题思路**

* 一开始，我们找出所有腐烂的橘子，将它们放入队列，作为第 0 层的结点。
* 然后进行 BFS 遍历，每个结点的相邻结点可能是上、下、左、右四个方向的结点，注意判断结点位于网格边界的特殊情况。
* 由于可能存在无法被污染的橘子，我们需要记录新鲜橘子的数量。在 BFS 中，每遍历到一个橘子（污染了一个橘子），就将新鲜橘子的数量减一。如果 BFS 结束后这个数量仍未减为零，说明存在无法被污染的橘子。


```python
class Solution(object):
    def orangesRotting(self, grid):
        q = deque([])
        # 记录新鲜橘子数量
        nums1 = 0

        m = len(grid)
        n = len(grid[0])
        # 找出所有腐烂橘子加入队列
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 2:
                    q.append((i, j))
                if grid[i][j] == 1:
                    nums1 += 1
        # 一开始就没有新鲜橘子，直接返回0
        if nums1 == 0:
            return 0
        # bfs
        t = -1
        while q:
            qsize = len(q)
            for _ in range(qsize):
                i, j = q.popleft()
                for di, dj in [[-1, 0], [1, 0], [0, 1], [0, -1]]:
                    newi = di + i
                    newj = dj + j
                    if 0 <= newi < m and 0<= newj <n and grid[newi][newj] == 1:
                        q.append((newi, newj))
                        grid[newi][newj] = 2
                        nums1 -= 1
            t += 1
        # bfs之后还有新鲜橘子返回-1                    
        return -1 if nums1 > 0 else t
```

**1162. 地图分析**

你现在手里有一份大小为 n x n 的 网格 grid，上面的每个 单元格 都用 0 和 1 标记好了。其中 0 代表海洋，1 代表陆地，请你找出一个海洋单元格，这个海洋单元格到离它最近的陆地单元格的距离是最大的。如果网格上只有陆地或者海洋，请返回 -1。

我们这里说的距离是「曼哈顿距离」（ Manhattan Distance）：(x0, y0) 和 (x1, y1) 这两个单元格之间的距离是 |x0 - x1| + |y0 - y1| 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/as-far-from-land-as-possible
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**解题思路**

* 稍微变换一下就是，寻找距离所有陆地最远的海洋。
* 所有陆地全部入队列，开始一圈圈bfs扩散，每次 +1 ，最后一次扩散到的0就是最远距离。

```python
class Solution(object):
    def maxDistance(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        m = len(grid)
        n = len(grid[0])
        visited = [[0] * n for _ in range(m)]
        q = collections.deque([])
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    q.append((i, j))
                    visited[i][j] = 1

        if not q or len(q) == m * n:
            return -1

        step = 0
        while q:
            for _ in range(len(q)):
                x, y = q.popleft()
                for dx, dy in [(-1, 0), (1, 0), (0, 1), (0, -1)]:
                    newx, newy = x + dx, y + dy
                    if 0<=newx<m and 0<=newy<n and visited[newx][newy] == 0:
                        visited[newx][newy] = 1
                        q.append([newx, newy])
            step += 1
        return step -1
```

**127. 单词接龙**

字典 wordList 中从单词 beginWord 和 endWord 的 转换序列 是一个按下述规格形成的序列 beginWord -> s1 -> s2 -> ... -> sk：

* 每一对相邻的单词只差一个字母。
*  对于 1 <= i <= k 时，每个 si 都在 wordList 中。注意， beginWord 不需要在 wordList 中。
* sk == endWord

给你两个单词 beginWord 和 endWord 和一个字典 wordList ，返回 从 beginWord 到 endWord 的 最短转换序列 中的 单词数目 。如果不存在这样的转换序列，返回 0 。

 
示例 1：

```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
输出：5
解释：一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog", 返回它的长度 5。
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/word-ladder
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**解题思路**

本题要求的是最短转换序列的长度，看到最短首先想到的就是广度优先搜索。想到广度优先搜索自然而然的就能想到图，但是本题并没有直截了当的给出图的模型，因此我们需要把它抽象成图的模型。

我们可以把每个单词都抽象为一个点，如果两个单词可以只改变一个字母进行转换，那么说明他们之间有一条双向边。因此我们只需要把满足转换条件的点相连，就形成了一张图。

基于该图，我们以 beginWord 为图的起点，以 endWord 为终点进行广度优先搜索，寻找 beginWord 到 endWord 的最短路径。

```python
class Solution(object):
    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: int
        """
        if endWord not in wordList:
            return 0

        h = {x: 1 for x in wordList}
        visited = {}
        q = collections.deque([beginWord])
        visited[beginWord] = 1

        step = 1
        wordLen = len(beginWord)

        while q:
            for _ in range(len(q)):
                word = q.popleft()
                # print(word, step)
                for i in range(wordLen):
                    for j in range(27):
                        newWord = word[:i] + chr(ord('a') + j) + word[i+1:]

                        if newWord == endWord:
                            return step + 1
                        if newWord in h and newWord not in visited:
                            visited[newWord] = 1
                            q.append(newWord)
            step += 1

        return 0
```

**433. 最小基因变化**

一条基因序列由一个带有8个字符的字符串表示，其中每个字符都属于 "A", "C", "G", "T"中的任意一个。

假设我们要调查一个基因序列的变化。一次基因变化意味着这个基因序列中的一个字符发生了变化。

例如，基因序列由"AACCGGTT" 变化至 "AACCGGTA" 即发生了一次基因变化。

与此同时，每一次基因变化的结果，都需要是一个合法的基因串，即该结果属于一个基因库。

现在给定3个参数 — start, end, bank，分别代表起始基因序列，目标基因序列及基因库，请找出能够使起始基因序列变化为目标基因序列所需的最少变化次数。如果无法实现目标变化，请返回 -1。

注意：

起始基因序列默认是合法的，但是它并不一定会出现在基因库中。
如果一个起始基因序列需要多次变化，那么它每一次变化之后的基因序列都必须是合法的。
假定起始基因序列与目标基因序列是不一样的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/minimum-genetic-mutation
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


**解题思路**

和单词接龙一样思路。

```python
class Solution(object):
    def minMutation(self, start, end, bank):
        """
        :type start: str
        :type end: str
        :type bank: List[str]
        :rtype: int
        """
        if end not in bank:
            return -1
        h = {x:1 for x in bank}
        visited = {}
        q = collections.deque([start])
        visited[start] = 1
        step = 0
        while q:
            for _ in range(len(q)):
                x = q.popleft()
                for i in range(len(x)):
                    for y in "ACGT":
                        newx = x[:i] + y + x[i+1:]
                        if newx == end:
                            return step + 1
                        if newx in h and newx not in visited:
                            visited[newx] = 1
                            q.append(newx)
            step += 1
        return -1
```

**752. 打开转盘锁**

你有一个带有四个圆形拨轮的转盘锁。每个拨轮都有10个数字： '0', '1', '2', '3', '4', '5', '6', '7', '8', '9' 。每个拨轮可以自由旋转：例如把 '9' 变为 '0'，'0' 变为 '9' 。每次旋转都只能旋转一个拨轮的一位数字。

锁的初始数字为 '0000' ，一个代表四个拨轮的数字的字符串。

列表 deadends 包含了一组死亡数字，一旦拨轮的数字和列表里的任何一个元素相同，这个锁将会被永久锁定，无法再被旋转。

字符串 target 代表可以解锁的数字，你需要给出解锁需要的最小旋转次数，如果无论如何不能解锁，返回 -1 。



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/open-the-lock
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


**解题思路**

和单词接龙一样思路。

```python
class Solution(object):
    def openLock(self, deadends, target):
        """
        :type deadends: List[str]
        :type target: str
        :rtype: int
        """
        dead = {x:1 for x in deadends}
        if target in dead or '0000' in dead:
            return -1
        if target == '0000':
            return 0
        

        visited = {}
        step = 0
        q = collections.deque(['0000'])
        visited['0000'] = 1
        while q:
            for _ in range(len(q)):
                x = q.popleft()
                for i in range(4):
                    for j in [str(int(x[i])-1), str(int(x[i])+ 1)]:
                        if j == '-1':
                            j = '9'
                        if j == '10':
                            j = '0'
                        newx = x[:i] + j + x[i+1:]
                        if newx == target:
                            return step + 1
                        
                        if newx not in dead and newx not in visited:
                            visited[newx] = 1
                            q.append(newx)
            step += 1
        return -1
```