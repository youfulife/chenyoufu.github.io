---
title: "733 图像渲染"
date: 2021-08-15T22:57:46+08:00
draft: false
tags: ["算法", "BFS", "DFS"]
categories: ["技术"]
---

**题目**

有一幅以二维整数数组表示的图画，每一个整数表示该图画的像素值大小，数值在 0 到 65535 之间。

给你一个坐标 (sr, sc) 表示图像渲染开始的像素值（行 ，列）和一个新的颜色值 newColor，让你重新上色这幅图像。

为了完成上色工作，从初始坐标开始，记录初始坐标的上下左右四个方向上像素值与初始坐标相同的相连像素点，接着再记录这四个方向上符合条件的像素点与他们对应四个方向上像素值与初始坐标相同的相连像素点，……，重复该过程。将所有有记录的像素点的颜色值改为新的颜色值。

最后返回经过上色渲染后的图像。

示例 1:
```
输入: 
image = [[1,1,1],[1,1,0],[1,0,1]]
sr = 1, sc = 1, newColor = 2
输出: [[2,2,2],[2,2,0],[2,0,1]]

解析: 
在图像的正中间，(坐标(sr,sc)=(1,1)),
在路径上所有符合条件的像素点的颜色都被更改成2。
注意，右下角的像素没有更改为2，
因为它不是在上下左右四个方向上与初始点相连的像素点。
```

链接：https://leetcode-cn.com/problems/flood-fill

**解题思路**

```python

BFS 先找到需要更新的点，然后再依次修改。31%

class Solution(object):
    def floodFill(self, image, sr, sc, newColor):
        rows = len(image)
        cols = len(image[0])

        bfs = [[sr, sc]]
        visited = set([(sr, sc)])
        for r0, c0 in bfs:
            for i, j in [[0, -1], [0, 1], [1, 0], [-1, 0]]:
                r = r0 + i
                c = c0 + j
                if 0 <= r < rows and 0 <= c < cols and image[r][c] == image[sr][sc] and (r, c) not in visited:
                    bfs.append([r, c])
                    visited.add((r, c))
        for i, j in visited:
            image[i][j] = newColor
        return image
```

优化下时间复杂度, 边遍历边修改，时间更优，99%

```python
class Solution(object):
    def floodFill(self, image, sr, sc, newColor):
        # 如果颜色不变，直接返回
        if image[sr][sc] == newColor:
            return image

        rows = len(image)
        cols = len(image[0])

        bfs = [[sr, sc]]
        # 这里要先保存起来
        old = image[sr][sc]

        for r0, c0 in bfs:
            # 修改为新的值
            image[r0][c0] = newColor
            for i, j in [[0, -1], [0, 1], [1, 0], [-1, 0]]:
                r = r0 + i
                c = c0 + j
                if 0 <= r < rows and 0 <= c < cols and image[r][c] == old:
                    bfs.append([r, c])

        return image
```