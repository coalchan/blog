---
title: Scala之旅——多参数列表（柯里化）
date: 2018-10-28 19:35:20
categories: [翻译]
tags: [scala]
description: 原文：https://docs.scala-lang.org/tour/multiple-parameter-lists.html
---

一个方法可以定义多参数列表。当调用一个不全的参数列表的方法时，会产生一个函数并将缺少的参数列表作为参数。这有个正式定义叫[柯里化](https://en.wikipedia.org/wiki/Currying)。

下面的例子定义在Scala集合中的[Traversable](/overviews/collections/trait-traversable.html)特质当中：

```
def foldLeft[B](z: B)(op: (B, A) => B): B
```

`foldLeft`按照从左往右的顺序，将二进制运算符`op`作用于初始值`z`和这个`traversable`中的所有元素。下面展示了它的用法。<!--more-->

从一个初始值0开始，`foldleft`将函数`(m, n) => m + n`作用于列表中的每个元素和上一次的累计值。

```tut
val numbers = List(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
val res = numbers.foldLeft(0)((m, n) => m + n)
print(res) // 55
```

多参数列表具有比较冗长的调用语法，因此要谨慎使用。建议使用的场景包括以下：

#### 单个函数参数

在单个函数参数的场景（如上面`foldleft`中的`op`）中，多参数列表允许使用简洁的语法将匿名函数传递给方法。如果没有多参数列表，代码将会是这样：

```
numbers.foldLeft(0, {(m: Int, n: Int) => m + n})
```

注意这里多参数列表的用法还可以让我们利用Scala类型推断来让代码变得更加简洁，这在不用柯里化来定义的情况下是做不到的。

```
numbers.foldLeft(0)(_ + _)
```

上面的语句`numbers.foldLeft(0)(_ + _)`允许我们固定参数`z`，然后传递一个偏函数进行使用，正如：

```tut
val numbers = List(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)
val numberFunc = numbers.foldLeft(List[Int]())_

val squares = numberFunc((xs, x) => xs:+ x*x)
print(squares.toString()) // List(1, 4, 9, 16, 25, 36, 49, 64, 81, 100)

val cubes = numberFunc((xs, x) => xs:+ x*x*x)
print(cubes.toString())  // List(1, 8, 27, 64, 125, 216, 343, 512, 729, 1000)
```

最后说下，`foldLeft`和`foldRight`可以使用下面任意一种用法来调用。

```tut
val numbers = List(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)

numbers.foldLeft(0)((sum, item) => sum + item) // Generic Form
numbers.foldRight(0)((sum, item) => sum + item) // Generic Form

numbers.foldLeft(0)(_+_) // Curried Form
numbers.foldRight(0)(_+_) // Curried Form

(0 /: numbers)(_+_) // Used in place of foldLeft
(numbers :\ 0)(_+_) // Used in place of foldRight
```

#### 隐式参数

如果要将参数列表中的某些参数指定为`implicit`，则应该使用多参数列表。如下所示：

```
def execute(arg: Int)(implicit ec: ExecutionContext) = ???
```