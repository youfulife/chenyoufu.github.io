---
title: "移动零"
date: 2021-04-09T22:57:43+08:00
draft: false
---

**题目**

给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

**示例:**

```
输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**说明:**

必须在原数组上操作，不能拷贝额外的数组。
尽量减少操作次数。

**链接：** https://leetcode-cn.com/problems/move-zeroes

**解题思路**

双指针，遇到非零的左右交换，左指针记录最左边0的位置。

**代码**

```go
func moveZeroes(nums []int)  {
    j := 0
    for i:=0;i<len(nums);i++ {
        if nums[i] != 0 {
            nums[i], nums[j] = nums[j], nums[i]
            j++
        }
    }
    return
}
```

