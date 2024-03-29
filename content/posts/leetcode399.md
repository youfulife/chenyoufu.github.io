---
title: "399 除法求值"
date: 2021-08-07T22:58:07+08:00
draft: false
tags: ["算法", "并查集", "五星"]
categories: ["技术"]
---

**题目**

给你一个变量对数组 equations 和一个实数值数组 values 作为已知条件，其中 equations[i] = [Ai, Bi] 和 values[i] 共同表示等式 Ai / Bi = values[i] 。每个 Ai 或 Bi 是一个表示单个变量的字符串。

另有一些以数组 queries 表示的问题，其中 queries[j] = [Cj, Dj] 表示第 j 个问题，请你根据已知条件找出 Cj / Dj = ? 的结果作为答案。

返回 所有问题的答案 。如果存在某个无法确定的答案，则用 -1.0 替代这个答案。如果问题中出现了给定的已知条件中没有出现的字符串，也需要用 -1.0 替代这个答案。

注意：输入总是有效的。你可以假设除法运算中不会出现除数为 0 的情况，且不存在任何矛盾的结果。

```
输入：equations = [["a","b"]], values = [0.5], queries = [["a","b"],["b","a"],["a","c"],["x","y"]]
输出：[0.50000,2.00000,-1.00000,-1.00000]
```

链接：https://leetcode-cn.com/problems/evaluate-division

**解题思路**

* 有思路，但是自己想不出来如何在并查集的构建过程中维护权值。
* 直接看官方题解的思路和答案。

```python
class Solution(object):
    class UFS(object):
        def __init__(self, n):
            self.count = n
            self.w = [1] * n
            self.parent = [i for i in range(n)]
        
        def find(self, x):
            if x != self.parent[x]:
                oldP = self.parent[x]
                self.parent[x] = self.find(self.parent[x])
                self.w[x] *= self.w[oldP] 
            return self.parent[x]
        
        def union(self, x, y, v):
            rootX = self.find(x)
            rootY = self.find(y)
            if rootX == rootY:
                return
            self.parent[rootX] = rootY
            self.w[rootX] = v * self.w[y] / self.w[x]
            return
        
        def isConnected(self, x, y):
            if self.find(x) == self.find(y):
                return self.w[x] / self.w[y]
            else:
                return -1

    def calcEquation(self, equations, values, queries):
        n = len(equations)
        ufs = Solution.UFS(n * 2)
        m = {}
        index = 0
        for i in range(n):
            c0 = equations[i][0]
            c1 = equations[i][1]
            if c0 not in m:
                m[c0] = index
                index += 1
            if c1 not in m:
                m[c1] = index
                index += 1
            ufs.union(m[c0], m[c1], values[i])
        
        ans = []
        for q in queries:
            if q[0] in m and q[1] in m:
                res = ufs.isConnected(m[q[0]], m[q[1]])
                ans.append(res)
            else:
                ans.append(-1)            
        return ans
```

官方题解的答案

链接：https://leetcode-cn.com/problems/evaluate-division/solution/399-chu-fa-qiu-zhi-nan-du-zhong-deng-286-w45d/

```java
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {

    public double[] calcEquation(List<List<String>> equations, double[] values, List<List<String>> queries) {
        int equationsSize = equations.size();

        UnionFind unionFind = new UnionFind(2 * equationsSize);
        // 第 1 步：预处理，将变量的值与 id 进行映射，使得并查集的底层使用数组实现，方便编码
        Map<String, Integer> hashMap = new HashMap<>(2 * equationsSize);
        int id = 0;
        for (int i = 0; i < equationsSize; i++) {
            List<String> equation = equations.get(i);
            String var1 = equation.get(0);
            String var2 = equation.get(1);

            if (!hashMap.containsKey(var1)) {
                hashMap.put(var1, id);
                id++;
            }
            if (!hashMap.containsKey(var2)) {
                hashMap.put(var2, id);
                id++;
            }
            unionFind.union(hashMap.get(var1), hashMap.get(var2), values[i]);
        }

        // 第 2 步：做查询
        int queriesSize = queries.size();
        double[] res = new double[queriesSize];
        for (int i = 0; i < queriesSize; i++) {
            String var1 = queries.get(i).get(0);
            String var2 = queries.get(i).get(1);

            Integer id1 = hashMap.get(var1);
            Integer id2 = hashMap.get(var2);

            if (id1 == null || id2 == null) {
                res[i] = -1.0d;
            } else {
                res[i] = unionFind.isConnected(id1, id2);
            }
        }
        return res;
    }

    private class UnionFind {

        private int[] parent;

        /**
         * 指向的父结点的权值
         */
        private double[] weight;


        public UnionFind(int n) {
            this.parent = new int[n];
            this.weight = new double[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                weight[i] = 1.0d;
            }
        }

        public void union(int x, int y, double value) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX == rootY) {
                return;
            }

            parent[rootX] = rootY;
          	// 关系式的推导请见「参考代码」下方的示意图
            weight[rootX] = weight[y] * value / weight[x];
        }

        /**
         * 路径压缩
         *
         * @param x
         * @return 根结点的 id
         */
        public int find(int x) {
            if (x != parent[x]) {
                int origin = parent[x];
                parent[x] = find(parent[x]);
                weight[x] *= weight[origin];
            }
            return parent[x];
        }

        public double isConnected(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);
            if (rootX == rootY) {
                return weight[x] / weight[y];
            } else {
                return -1.0d;
            }
        }
    }
}
```