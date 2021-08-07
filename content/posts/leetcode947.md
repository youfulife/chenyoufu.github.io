---
title: "947 移除最多的同行或同列石头"
date: 2021-08-07T12:31:41+08:00
draft: false
tags: ["算法", "并查集", "五星"]
categories: ["技术"]
---

**题目**

n 块石头放置在二维平面中的一些整数坐标点上。每个坐标点上最多只能有一块石头。

如果一块石头的 同行或者同列 上有其他石头存在，那么就可以移除这块石头。

给你一个长度为 n 的数组 stones ，其中 stones[i] = [xi, yi] 表示第 i 块石头的位置，返回 可以移除的石子 的最大数量。


示例 1：
```
输入：stones = [[0,0],[0,1],[1,0],[1,2],[2,1],[2,2]]
输出：5
解释：一种移除 5 块石头的方法如下所示：
1. 移除石头 [2,2] ，因为它和 [2,1] 同行。
2. 移除石头 [2,1] ，因为它和 [0,1] 同列。
3. 移除石头 [1,2] ，因为它和 [1,0] 同行。
4. 移除石头 [1,0] ，因为它和 [0,0] 同列。
5. 移除石头 [0,1] ，因为它和 [0,0] 同行。
石头 [0,0] 不能移除，因为它没有与另一块石头同行/列。
```

链接：https://leetcode-cn.com/problems/most-stones-removed-with-same-row-or-column

**解题思路**

并查集

* 将同一行或者同一列的石头连通起来
* 每个连通分量中最后只需要保留一块石头即可
* 所以，最后移除石头的数量 = 总的石头数 - 连通分量的个数 

```python
class Solution(object):
    class UFS(object):
        def __init__(self, n):
            self.count = n
            self.p = [i for i in range(n)]
        def find(self, x):
            if x != self.p[x]:
                self.p[x] = self.find(self.p[x])
            return self.p[x]
        def union(self, x, y):
            x_root = self.find(x)
            y_root = self.find(y)
            if x_root == y_root:
                return
            self.p[x_root] = y_root
            self.count -= 1
            return

    def removeStones(self, stones):
        n = len(stones)
        ufs = Solution.UFS(n)
        for i in range(n):
            for j in range(i+1, n):
                # 判断两块石头是不是属于同一行或同一列
                if stones[i][0] == stones[j][0] or stones[i][1] == stones[j][1]:
                    ufs.union(i, j)
        return n - ufs.count
```

优化代码复杂度

「合并」的语义
「合并」的语义是：所有横坐标为 x 的石头和所有纵坐标为 y 的石头都属于同一个连通分量。
那如果以行和列作为基础，时间复杂度是可以降低的：

* 将行和列转化到同一个维度（也就是说将行和列仅仅当作一个数字就行）
* 当我们遍历到一个点(x, y)时，直接将x与y进行合并（说明该行和该列行的所有点都属于同一个并查集）
* 最后用stones的大小减去并查集的个数即可
* 但是，x和y的值可能冲突，所以这里我们将x加上10001（题目范围限定为10000）
* 在并查集里我们需要维护连通分量的个数，新创建顶点的时候连通分量加 1；合并不在同一个连通分量中的两个并查集的时候，连通分量减 1。
* 采用hashmap来存储每个元素

```python
class Solution(object):
    class UFS(object):
        def __init__(self):
            self.count = 0
            self.p = {}
        def find(self, x):
            if x not in self.p.keys():
                self.count += 1
                self.p[x] = x
            if x != self.p[x]:
                self.p[x] = self.find(self.p[x])
            return self.p[x]
        def union(self, x, y):
            x_root = self.find(x)
            y_root = self.find(y)
            if x_root == y_root:
                return
            self.p[x_root] = y_root
            self.count -= 1
            return

    def removeStones(self, stones):
        n = len(stones)
        ufs = Solution.UFS()
        for store in stones:
            ufs.union(store[0] + 10001, store[1])
        return n - ufs.count
```