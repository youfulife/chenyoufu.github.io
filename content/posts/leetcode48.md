---
title: "48 旋转图像"
date: 2021-06-18T23:06:28+08:00
draft: false
tags: ["算法", "二维数组"]
categories: ["技术"]
---
**题目**

给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[[7,4,1],[8,5,2],[9,6,3]]
```

链接：https://leetcode-cn.com/problems/rotate-image

**解题思路**

* 旋转后 matrix[col][n-row-1] = matrix[row][col]
* 引入一个辅助数组

```python
class Solution(object):
    def rotate(self, matrix):
        """
        :type matrix: List[List[int]]
        :rtype: None Do not return anything, modify matrix in-place instead.
        """
        n = len(matrix)
        temp = [[0 for i in range(n)] for j in range(n)]
        for i in range(n):
            for j in range(n):
                temp[j][n-i-1] = matrix[i][j]
        
        for i in range(n):
            for j in range(n):
                matrix[i][j] = temp[i][j]
        return
```

**不使用辅助数组**

* 旋转后 matrix[col][n-row-1] = matrix[row][col]
* 先水平反转 matrix[row][col] = matrix[n-row-1][col]
* 再对角线反转 matrix[col][n-row-1] = matrix[n-row-1][col] = matrix[row][col]

```python
class Solution(object):
    def rotate(self, matrix):
        n = len(matrix)
        # 先水平反转
        for i in range(n/2):
            for j in range(n):
                matrix[i][j], matrix[n-i-1][j] = matrix[n-i-1][j], matrix[i][j]
        # 再对角线反转
        for i in range(n):
            for j in range(i, n): # 注意j的范围
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
        return
```