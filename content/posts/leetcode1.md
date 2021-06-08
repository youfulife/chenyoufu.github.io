---
title: "1 两数之和"
date: 2021-06-07T22:25:32+08:00
draft: false
tags: ["算法", "数组"]
categories: ["技术"]
---

**题目**
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

示例 1：
```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

链接：https://leetcode-cn.com/problems/two-sum

**解题思路**

hash表

```go
func twoSum(nums []int, target int) []int {

    // [] 2 -> nil
    // [1] 3 -> nil
    // [2, 3] 5 -> [0, 1]
    // [3, 5, 1, 2] 8 -> [0, 1]

    if len(nums) < 2 {
        return nil
    }

    var m = make(map[int]int)
    for i, num := range nums {
        v, ok := m[target-num]
        if ok {
            return []int{v, i}
        }
        m[num] = i
    }
    return nil
}
```