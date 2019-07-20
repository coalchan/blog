---
title: 操作符
date: 2018-07-11 23:59:00
categories: [技术]
tags: [scala]
---
## 操作符即方法

例如`1 + 2`实际上执行的是`1.+(2)`：<!--more-->

```scala
scala> val sum = 1 + 2
sum: Int = 3
```

为了证明这点，可以：

```scala
scala> val sum = 1.+(2)
sum: Int = 3
```

事实上，`Int`包含了多个重载的`+`方法，分别接受不同的参数类型，比如

```scala
scala> val longSum = 1 + 2L
longSum: Long = 3
```

### 以冒号结尾的

如果操作符以冒号（`:`）结尾，则该方法的调用发生在右操作元（right operand）上。
例如`1 :: Nil`表示的是`Nil.::(1)`

上面是**中缀操作符**，另外还有两种：**前缀操作符**和**后缀操作符**，区别在于接收的操作元是一元的（unary）。

## 前缀操作符

唯一能被用作前缀操作符的是`+`、`-`、`!`和`~`。

例如`Int`中的前缀操作符：

```scala
scala> -2 // 实际上调用的 2.unary_-
res1: Int = -2
```

## 后缀操作符

一般对于无参的方法，可以选择去掉`.`的方式，使用后缀表达式：

```scala
scala> "aBc" toLowerCase
res2: String = abc
```

## `==`操作符：对象相等性

`==`在Scala中与Java不同的在于引用类型的比较上，Java比较的是指向同一个对象，而Scala比较的是`equals`方法的执行结果。常用的是List的比较：

```scala
scala> List(1, 2) == List(1, 2)
res3: Boolean = true
```

另外Scala也提供了比较引用相同的机制，即名为`eq`的方法。