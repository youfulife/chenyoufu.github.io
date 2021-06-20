---
title: "36 有效的数独"
date: 2021-06-20T11:04:34+08:00
draft: false
tags: ["算法", "数组"]
categories: ["技术"]
---
**题目**

请你判断一个 9x9 的数独是否有效。只需要 根据以下规则 ，验证已经填入的数字是否有效即可。

* 数字 1-9 在每一行只能出现一次。
* 数字 1-9 在每一列只能出现一次。
* 数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。（请参考示例图）

数独部分空格内已填入了数字，空白格用 '.' 表示。

注意：

* 一个有效的数独（部分已被填充）不一定是可解的。
* 只需要根据以上规则，验证已经填入的数字是否有效即可。

链接：https://leetcode-cn.com/problems/valid-sudoku

**解题思路**

一个简单的解决方案是遍历该 9 x 9 数独 三 次，以确保：

* 行中没有重复的数字。
* 列中没有重复的数字。
* 3 x 3 子数独内没有重复的数字。

实际上，所有这一切都可以在一次迭代中完成。

枚举子数独, 可以使用 box_index = (row / 3) * 3 + columns / 3，其中 / 是整数除法。

```python
class Solution(object):
    def isValidSudoku(self, board):
        drow = [{} for i in range(9)]
        dcol = [{} for i in range(9)]
        dbox = [{} for i in range(9)]
        for i in range(9):
            for j in range(9):
                v = board[i][j]
                if v == '.':
                    continue
                boxId = i/3 * 3 + j/3
                if v in drow[i] or v in dcol[j] or v in dbox[boxId]:
                    return False
                drow[i][v] = 1
                dcol[j][v] = 1
                dbox[boxId][v] = 1
        return True
```