---
title: "Pandas Learning With Chatgpt"
date: 2023-03-30T15:42:19+08:00
draft: false
tags: ["chatgpt", "pandas", "python", "数据分析"]
categories: ["技术"]
---


**在pandas中给一个df增加新的列有哪些方法?**

在 Pandas 中，我们可以使用以下方法向一个 DataFrame 添加新列：

1. 直接赋值：可以直接使用赋值语句来给 DataFrame 添加新列，例如：

   ```
   df['new_col'] = [1, 2, 3, 4, 5]
   ```

2. 使用 insert() 方法：使用 insert() 方法可以在 DataFrame 中插入一个新的列，例如：

   ```
   df.insert(loc=2, column='new_col', value=[1, 2, 3, 4, 5])
   ```

   这里的 loc 参数指定了要插入的列的位置，column 参数指定了新列的名称，value 参数指定了新列的值。

3. 使用 assign() 方法：使用 assign() 方法可以基于已有的列创建新的列，例如：

   ```
   df.assign(new_col=[1, 2, 3, 4, 5])
   ```

   这里的 new_col 参数指定了新列的名称，后面的列表指定了新列的值。

4. 使用 eval() 方法：使用 eval() 方法可以基于已有的列创建新的列，例如：

   ```
   df.eval('new_col = col1 + col2')
   ```

   这里的 new_col 参数指定了新列的名称，后面的表达式使用了已有的列 col1 和 col2 来计算新列的值。注意，eval() 方法中的表达式必须使用字符串类型传递。

以上是一些常见的方法，除此之外还有其他的方法，具体可以参考 Pandas 的文档。

