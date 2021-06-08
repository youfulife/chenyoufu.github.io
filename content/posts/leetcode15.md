---
title: "三数之和"
date: 2021-04-08T22:11:56+08:00
draft: false
tags: ["算法", "数组"]
categories: ["技术"]
---

题目：给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

**注意**：答案中不可以包含重复的三元组。

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
```

**示例 2：**

```
输入：nums = []
输出：[]
```

**示例 3：**

```
输入：nums = [0]
输出：[]
```

**提示：**

- 0 <= nums.length <= 3000
- -105 <= nums[i] <= 105


链接：https://leetcode-cn.com/problems/3sum


**解题思路**

1. 先从小到大排序，双指针，先固定住一个 i，然后两个指针 k 和 j 分别指向i的右侧和数组的最后一个元素。
2. 判断 nums[i]+ nums[k] + nums[j] 如果等于零，两个指针都向内移动，如果大于0，大的往左移动，让和变小一些，如果小于0，小的往右移动，让和变大一些，直到两个指针相遇。
3. 移动过程中注意3个地方的去重。



**代码**

```go
func threeSum(nums []int) [][]int {
    var ans [][]int
    sort.Ints(nums)

    for i:=0;i<len(nums);i++ {

        if i > 0 && nums[i] == nums[i-1]{
            continue
        }

        k := i+1
        j := len(nums) - 1

        for k < j {
            if k > i+1 && nums[k] == nums[k-1] {
                k++
                continue
            }

            if j < len(nums) - 1 && nums[j] == nums[j+1] {
                j--
                continue
            }

            if nums[i]+ nums[k] + nums[j] == 0 {
                ans = append(ans, []int{nums[i], nums[k], nums[j]})
                k++
                j--
            } else if nums[i]+ nums[k] + nums[j] > 0 {
                j--
            } else {
                k++
            }
        }
    }
    return ans
}
```



**解题后记**

2021.04.08

第一次做是按照两个数之和的思路做的，先固定住一个数，然后将问题转变为2个数之和，用hash表的方法，但是在做的过程中发现去重很麻烦，直接看题解。

后来过了两个星期，发现再做又变陌生了，做不出来了，看了一下之前提交通过的答案，然后又自己写了一遍，今天又自己写了一遍。

目前这个题是第三遍了，感觉还是远远不够。

![](/img/1617893528206.jpg)


----

2021.06.07

```python
def threeSum(self, nums):
        ans = []
        nums = sorted(nums)
        for i in range(len(nums)):
            if i > 0 and nums[i] == nums[i-1]:
                continue
            
            k = i+1
            j = len(nums) - 1
            while k<j:
                if k > i+1 and nums[k] == nums[k-1]:
                    k += 1
                    continue
                if j < len(nums) - 1 and nums[j] == nums[j+1]:
                    j -= 1
                    continue
                if nums[i] + nums[k] + nums[j] == 0:
                    ans.append([nums[i], nums[k], nums[j]])
                    k += 1 # 这里总是忘记
                    j -= 1
                elif nums[i] + nums[k] + nums[j] > 0:
                    j -= 1
                else:
                    k += 1
        return ans
```