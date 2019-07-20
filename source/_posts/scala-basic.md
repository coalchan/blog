---
title: Scala基础
date: 2018-07-10 13:04:43
categories: [技术]
tags: [scala]
description: Scala基础，从现在开始~
---
## 声明变量
主要有两种方式：`val`和`var`
`val x = 2 * 4 + 1`
`val`命名的变量不可修改，否则会报错。

Scala鼓励开发者使用`val`，而事实也发现大多数程序并不需要太多的可修改变量`var`。
同时，也可以指定类型：
`val x: String = "Hello, Scala"`
这里可以看出Scala提供了两种方式供开发选择，在实际使用中，建议在简单的程序中为了代码的简介可以考虑去掉类型，而在相对复杂的大型工程中，建议在某些地方加上类型，提高可读性。<!--more-->

## 操作符
由于scala中一切操作都是方法调用，当一个方法调用以操作符的形式出现的时候，比如:
`a * b`表示的其实是`a.*(b)`，即方法的调用默认发生在左操作元（left operand）上。
但是，如果操作符以冒号（`:`）结尾，则该方法的调用发生在右操作元（right operand）上。
例如`1 :: Nil`表示的是`Nil.::(1)`

另外，Scala并未提供`++`和`--`操作符，因为`Int`类是不可变的，很难不通过重新赋值（`=`），而实现`++`去改变整形的值。

## apply方法
在Scala中，有种类似函数的调用方式，如`"abc"(2) // 得到C`。
这个背后的实现原理是调用了`apply`方法，这里调用的是`StringOps`中的：
`override def apply(index: Int): Char = repr charAt index`

