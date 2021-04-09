---
title: "爬楼梯"
date: 2021-04-09T22:34:00+08:00
draft: false
---
**题目**

假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

**注意：**给定 n 是一个正整数。

**示例 1：**

```
输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。

1.  1 阶 + 1 阶
2.  2 阶
```

**示例 2：**

```
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。

1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

**链接：** https://leetcode-cn.com/problems/climbing-stairs

**解题思路**

f(n) = f(n-1) + f(n-2) 其实就是斐波那契数列。



第一遍自己写的代码。

```go
func climbStairs(n int) int {
    if n == 1 || n == 2{
        return n
    }

    f1 := 1
    f2 := 2
    for i:=3; i <= n; i++ {
        f3 := f1 + f2
        f1 = f2
        f2 = f3
    }
    return f2
}
```

看了英文站评论区投票的答案后优化一下。

```go
func climbStairs(n int) int {
    a := 1
    b := 1
    for i := 0; i < n; i++ {
        a, b = b, a + b
    }
    return a
}
```

各种语言的短小代码。

https://leetcode.com/problems/climbing-stairs/discuss/25296/3-4-short-lines-in-every-language

**提交记录**

![image-20210409224300797](/img/image-20210409224300797.png)