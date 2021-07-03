---
title: "278 第一个错误的版本"
date: 2021-05-29T21:49:39+08:00
draft: false
tags: ["算法", "数组", "第一个"]
categories: ["技术"]
---

**题目**

你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 n 个版本 [1, 2, ..., n]，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用 bool isBadVersion(version) 接口来判断版本号 version 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

示例:
```
给定 n = 5，并且 version = 4 是第一个错误的版本。

调用 isBadVersion(3) -> false
调用 isBadVersion(5) -> true
调用 isBadVersion(4) -> true

所以，4 是第一个错误的版本。 
```

链接：https://leetcode-cn.com/problems/first-bad-version

**解题思路**

二分查找

```go
func firstBadVersion(n int) int {
    left := 1
    right := n
    
    for left != right && left != right - 1  {
        mid := (left + right) / 2
        if isBadVersion(mid) {
            right = mid
        } else {
            left = mid
        }
    }
    if isBadVersion(left) {
        return left
    }
    return right
}
```

优化后

```go
func firstBadVersion(n int) int {
    left := 1
    right := n
    
    for left < right  {
        mid := left + (right - left) / 2 // 可以避免溢出
        if isBadVersion(mid) {
            right = mid
        } else {
            left = mid + 1
        }
    }
    return left
}

```

----

2021.07.03 二刷 

二分查找左边界，发现二分查找的三种方式（左边界，任意点，右边界）又有点遗忘了，继续巩固。


