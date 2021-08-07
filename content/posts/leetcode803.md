---
title: "803 打砖块"
date: 2021-08-07T16:32:53+08:00
draft: false
tags: ["算法", "并查集", "五星", "逆向思维"]
categories: ["技术"]
---

**题目**

有一个 m x n 的二元网格，其中 1 表示砖块，0 表示空白。砖块 稳定（不会掉落）的前提是：

一块砖直接连接到网格的顶部，或者
至少有一块相邻（4 个方向之一）砖块 稳定 不会掉落时
给你一个数组 hits ，这是需要依次消除砖块的位置。每当消除 hits[i] = (rowi, coli) 位置上的砖块时，对应位置的砖块（若存在）会消失，然后其他的砖块可能因为这一消除操作而掉落。一旦砖块掉落，它会立即从网格中消失（即，它不会落在其他稳定的砖块上）。

返回一个数组 result ，其中 result[i] 表示第 i 次消除操作对应掉落的砖块数目。

注意，消除可能指向是没有砖块的空白位置，如果发生这种情况，则没有砖块掉落。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/bricks-falling-when-hit
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

**解题思路**

**并查集**

自己没有思路，根据官方题解的思路，调试修改了好几处才通过。

* 消失的数量 = 击碎之前与顶部连通的数量 - 击碎之后连通的数量 - 1，1 其实就表示当前被击碎的砖块。
* 连通分量的数量，可以基于并查集的秩优化策略，其中秩表示子节点的数量，而不是通常的表示树的高度。
* 逆向思维，把砖块补上去，通过计算补上去之后的数量 - 补上去之前的数量 , 这个太难想了。
* 设置一个特殊的dummy节点，表示屋顶，可以先把第一行的所有为1的节点连通到屋顶。

如何使用并查集

* 消除一个砖块的效果是：一个连通分量被分成了两个连通分量；
* 并查集的作用是：把两个连通分量合并成一个连通分量。

提示我们这个问题需要 反向 思考。即考虑：补上被击碎的砖块以后，有多少个砖块因为这个补上的这个砖块而与屋顶的砖块相连。每一次击碎一个砖块，因击碎砖块而消失的砖块只会越来越少。因此可以按照数组 hits 的顺序 逆序地 把这些砖块依次补上。




```python
class Solution(object):
    class UFS(object):
        def __init__(self, n):
            self.dummy = 0
            self.count = n
            self.p = [i for i in range(n)]
            self.rank = [1] * n
        def find(self, x):
            while x != self.p[x]:
                x = self.p[x]
            return x
        def union(self, x, y):
            x_root = self.find(x)
            y_root = self.find(y)
            if x_root == y_root:
                return
            if self.rank[x_root] < self.rank[y_root]:
                self.p[x_root] = y_root
                self.rank[y_root] += self.rank[x_root]
            else:
                self.p[y_root] = x_root
                self.rank[x_root] += self.rank[y_root]

            self.count -= 1
            return
    def hitBricks(self, grid, hits):
        m = len(grid)
        n = len(grid[0])
        ufs = Solution.UFS(m * n + 1)
        for h in hits:
            grid[h[0]][h[1]] -= 1
        dummy = m * n
        for i in [0]:
            for j in range(n):
                if grid[i][j] == 1:
                    ufs.union(dummy, i * n + j)

        for i in range(m):
            for j in range(n):
                if grid[i][j] != 1:
                    continue
                x = i * n + j
                if i < m-1 and grid[i+1][j] == 1:
                    y = (i+1) * n + j
                    ufs.union(x, y)
                if j < n-1 and grid[i][j+1] == 1:
                    y = i * n + j + 1
                    ufs.union(x, y)
        ans = []
        while hits:
            h = hits.pop()
            i, j = h[0], h[1]
            grid[i][j] += 1
            if grid[i][j] != 1:
                ans.append(0)
                continue
            x = i * n + j
            t0 = ufs.rank[ufs.find(dummy)]
            if i == 0:
                ufs.union(x, dummy)
            if i > 0 and grid[i-1][j] == 1:
                y = (i-1) * n + j
                ufs.union(x, y)
            if i < m-1 and grid[i+1][j] == 1:
                y = (i+1) * n + j
                ufs.union(x, y)
            if j > 0 and grid[i][j-1] == 1:
                y = i * n + j - 1
                ufs.union(x, y)
            if j < n-1 and grid[i][j+1] == 1:
                y = i * n + j + 1
                ufs.union(x, y)
            t1 = ufs.rank[ufs.find(dummy)]
            if t1 > t0:
                ans.append(t1 - t0 - 1)
            else:
                ans.append(0)
        return ans[::-1]
```

优化下代码

```python
class Solution(object):
    class UFS(object):
        def __init__(self, n):
            self.p = [i for i in range(n)]
            self.rank = [1] * n
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
            self.rank[y_root] += self.rank[x_root]
            return
        def size(self, x):
            return self.rank[self.find(x)]
    
    def hitBricks(self, grid, hits):

        def getIndex(i, j):
            return i * n + j

        m = len(grid)
        n = len(grid[0])
        ufs = Solution.UFS(m * n + 1)
        for h in hits:
            grid[h[0]][h[1]] -= 1
        
        # 屋顶
        dummy = m * n
        for j in range(n):     
            if grid[0][j] == 1:  
                ufs.union(dummy, j)

        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    # 只需要往右和往下看
                    dirs = [(1, 0), (0, 1)]
                    for di, dj in dirs:
                        newI, newJ = i + di, j + dj
                        if 0 <= newI < m and 0 <= newJ < n and grid[newI][newJ] == 1:
                            ufs.union(getIndex(i, j), getIndex(newI, newJ))

        ans = []
        while hits:
            h = hits.pop()
            i, j = h[0], h[1]
            grid[i][j] += 1
            if grid[i][j] != 1:
                ans.append(0)
                continue
            x = i * n + j
            t0 = ufs.size(dummy)
            if i == 0:
                ufs.union(x, dummy)
            dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]
            for di, dj in dirs:
                newI, newJ = i + di, j + dj
                if 0 <= newI < m and 0 <= newJ < n and grid[newI][newJ] == 1:
                    ufs.union(getIndex(i, j), getIndex(newI, newJ))
            t1 = ufs.size(dummy)
            ans.append(max(0, t1 - t0 - 1))
            
        return ans[::-1]
```

官方题解

https://leetcode-cn.com/leetbook/read/disjoint-set/owl1lv/

```java
public class Solution {

    private int rows;
    private int cols;

    public static final int[][] DIRECTIONS = {{0, 1}, {1, 0}, {-1, 0}, {0, -1}};

    public int[] hitBricks(int[][] grid, int[][] hits) {
        this.rows = grid.length;
        this.cols = grid[0].length;

        // 第 1 步：把 grid 中的砖头全部击碎，通常算法问题不能修改输入数据，这一步非必需，可以认为是一种答题规范
        int[][] copy = new int[rows][cols];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                copy[i][j] = grid[i][j];
            }
        }

        // 把 copy 中的砖头全部击碎
        for (int[] hit : hits) {
            copy[hit[0]][hit[1]] = 0;
        }

        // 第 2 步：建图，把砖块和砖块的连接关系输入并查集，size 表示二维网格的大小，也表示虚拟的「屋顶」在并查集中的编号
        int size = rows * cols;
        UnionFind unionFind = new UnionFind(size + 1);

        // 将下标为 0 的这一行的砖块与「屋顶」相连
        for (int j = 0; j < cols; j++) {
            if (copy[0][j] == 1) {
                unionFind.union(j, size);
            }
        }

        // 其余网格，如果是砖块向上、向左看一下，如果也是砖块，在并查集中进行合并
        for (int i = 1; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (copy[i][j] == 1) {
                    // 如果上方也是砖块
                    if (copy[i - 1][j] == 1) {
                        unionFind.union(getIndex(i - 1, j), getIndex(i, j));
                    }
                    // 如果左边也是砖块
                    if (j > 0 && copy[i][j - 1] == 1) {
                        unionFind.union(getIndex(i, j - 1), getIndex(i, j));
                    }
                }
            }
        }

        // 第 3 步：按照 hits 的逆序，在 copy 中补回砖块，把每一次因为补回砖块而与屋顶相连的砖块的增量记录到 res 数组中
        int hitsLen = hits.length;
        int[] res = new int[hitsLen];
        for (int i = hitsLen - 1; i >= 0; i--) {
            int x = hits[i][0];
            int y = hits[i][1];

            // 注意：这里不能用 copy，语义上表示，如果原来在 grid 中，这一块是空白，这一步不会产生任何砖块掉落
            // 逆向补回的时候，与屋顶相连的砖块数量也肯定不会增加
            if (grid[x][y] == 0) {
                continue;
            }

            // 补回之前与屋顶相连的砖块数
            int origin = unionFind.getSize(size);

            // 注意：如果补回的这个结点在第 1 行，要告诉并查集它与屋顶相连（逻辑同第 2 步）
            if (x == 0) {
                unionFind.union(y, size);
            }

            // 在 4 个方向上看一下，如果相邻的 4 个方向有砖块，合并它们
            for (int[] direction : DIRECTIONS) {
                int newX = x + direction[0];
                int newY = y + direction[1];

                if (inArea(newX, newY) && copy[newX][newY] == 1) {
                    unionFind.union(getIndex(x, y), getIndex(newX, newY));
                }
            }

            // 补回之后与屋顶相连的砖块数
            int current = unionFind.getSize(size);
            // 减去的 1 是逆向补回的砖块（正向移除的砖块），与 0 比较大小，是因为存在一种情况，添加当前砖块，不会使得与屋顶连接的砖块数更多
            res[i] = Math.max(0, current - origin - 1);

            // 真正补上这个砖块
            copy[x][y] = 1;
        }
        return res;
    }

    /**
     * 输入坐标在二维网格中是否越界
     *
     * @param x
     * @param y
     * @return
     */
    private boolean inArea(int x, int y) {
        return x >= 0 && x < rows && y >= 0 && y < cols;
    }

    /**
     * 二维坐标转换为一维坐标
     *
     * @param x
     * @param y
     * @return
     */
    private int getIndex(int x, int y) {
        return x * cols + y;
    }

    private class UnionFind {

        /**
         * 当前结点的父亲结点
         */
        private int[] parent;
        /**
         * 以当前结点为根结点的子树的结点总数
         */
        private int[] size;

        public UnionFind(int n) {
            parent = new int[n];
            size = new int[n];
            for (int i = 0; i < n; i++) {
                parent[i] = i;
                size[i] = 1;
            }
        }

        /**
         * 路径压缩，只要求每个不相交集合的「根结点」的子树包含的结点总数数值正确即可，因此在路径压缩的过程中不用维护数组 size
         *
         * @param x
         * @return
         */
        public int find(int x) {
            if (x != parent[x]) {
                parent[x] = find(parent[x]);
            }
            return parent[x];
        }

        public void union(int x, int y) {
            int rootX = find(x);
            int rootY = find(y);

            if (rootX == rootY) {
                return;
            }
            parent[rootX] = rootY;
            // 在合并的时候维护数组 size
            size[rootY] += size[rootX];
        }

        /**
         * @param x
         * @return x 在并查集的根结点的子树包含的结点总数
         */
        public int getSize(int x) {
            int root = find(x);
            return size[root];
        }
    }
}
```