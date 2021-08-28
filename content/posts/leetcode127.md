---
title: "127 单词接龙"
date: 2021-08-28T18:34:59+08:00
draft: false
tags: ["算法", "图", "BFS", "五星"]
categories: ["技术"]
---

**题目**

字典 wordList 中从单词 beginWord 和 endWord 的 转换序列 是一个按下述规格形成的序列：

* 序列中第一个单词是 beginWord 。
* 序列中最后一个单词是 endWord 。
* 每次转换只能改变一个字母。
* 转换过程中的中间单词必须是字典 wordList 中的单词。

给你两个单词 beginWord 和 endWord 和一个字典 wordList ，找到从 beginWord 到 endWord 的 最短转换序列 中的 单词数目 。如果不存在这样的转换序列，返回 0。

 
示例 1：
```
输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
输出：5
解释：一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog", 返回它的长度 5。
```

链接：https://leetcode-cn.com/problems/word-ladder

**解题思路**

本题要求的是最短转换序列的长度，看到最短首先想到的就是广度优先搜索。想到广度优先搜索自然而然的就能想到图，但是本题并没有直截了当的给出图的模型，因此我们需要把它抽象成图的模型。

我们可以把每个单词都抽象为一个点，如果两个单词可以只改变一个字母进行转换，那么说明他们之间有一条双向边。因此我们只需要把满足转换条件的点相连，就形成了一张图。

基于该图，我们以 beginWord 为图的起点，以 endWord 为终点进行广度优先搜索，寻找 beginWord 到 endWord 的最短路径。

```python
class Solution(object):
    def ladderLength(self, beginWord, endWord, wordList):
        if beginWord == endWord:
            return 1
        if endWord not in wordList:
            return 0
        
        d = {x: 1 for x in wordList if x != beginWord}

        q = deque([beginWord])
        step = 1

        while q:
            qsize = len(q)
            for _ in range(qsize):
                word = q.popleft()
                
                for i in range(len(word)):
                    a = list(word)
                    for j in range(26):
                        a[i] = chr(ord('a') + j)
                        b = ''.join(a)

                        if b == endWord:
                            return step + 1

                        if b in d and d[b] == 1:
                            q.append(b)
                            d[b] = 0
            step += 1
        return 0
```

优化下时间

```python

class Solution(object):

    def replaceWord(self, word, m):
        ans = []
        for i in range(len(word)):
            for x in m[i]:
                ans.append(word[:i] + x + word[i+1:])
        return ans


    def ladderLength(self, beginWord, endWord, wordList):
        if beginWord == endWord:
            return 1
        if endWord not in wordList:
            return 0
        
        d = {x: 1 for x in wordList if x != beginWord}
        
        n = len(beginWord)
        selected = [set() for _ in range(n)]
        for word in wordList:
            for i in range(n):
                selected[i].add(word[i])

        q = deque([beginWord])
        step = 1

        while q:
            qsize = len(q)
            for _ in range(qsize):
                word = q.popleft()

                replaced = self.replaceWord(word, selected)
                for x in replaced:
                    if x == endWord:
                        return step + 1
                    if d.get(x, 0) == 1:
                        q.append(x)
                        d[x] = 0
            step += 1
        return 0
```