---
title: JavaScript著名的变量命名规则
date: 2018-03-07 23:47:21
updated: 2019-02-11 17:31:52
comments: 1
categories: JavaScript
tags: []
permalink: /post/19.html
---

这些命名方法不仅限于 JavaScript，在其它任何编程语言也比较适用。

只是因为变量名的语法正确，并不意味着就该使用它们。变量还应遵守以下某条著名的命名规则：

<!--more-->

### Camel 标记法

首字母是小写的，接下来的字母都以大写字符开头。例如：
var myTestValue = 0, mySecondeValue = "hi";

### Pascal 标记法

首字母是大写的，接下来的字母都以大写字符开头。例如：
var MyTestValue = 0, MySecondeValue = "hi";

### 匈牙利类型标记法

在以 Pascal 标记法命名的变量前附加一个小写字母（或小写字母序列），说明该变量的类型。例如，i 表示整数，s 表示字符串，如下所示：
var iMyTestValue = 1, sMySecondeValue = "hi";

#### 参考 w3school 的前缀表

| 类型                   | 前缀 | 示例      |
| ---------------------- | ---- | --------- |
| 数组                   | a    | aValues   |
| 布尔型                 | b    | bFound    |
| 浮点型（数字）         | f    | fValue    |
| 函数                   | fn   | fnMethod  |
| 整型（数字）           | i    | iValue    |
| 对象                   | o    | oType     |
| 正则表达式             | re   | rePattern |
| 字符串                 | s    | sValue    |
| 变型（可以是任何类型） | v    | vValue    |
