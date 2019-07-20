---
title: Scala之旅——嵌套方法
date: 2018-08-12 22:06:18
categories: [翻译]
tags: [scala]
description: 原文：https://docs.scala-lang.org/tour/nested-functions.html
---

在Scala中，可以定义嵌套方法。下面的例子中，提供了一个方法`factorial`用于计算给定数字的阶乘：

```tut
 def factorial(x: Int): Int = {
    def fact(x: Int, accumulator: Int): Int = {
      if (x <= 1) accumulator
      else fact(x - 1, x * accumulator)
    }  
    fact(x, 1)
 }

 println("Factorial of 2: " + factorial(2))
 println("Factorial of 3: " + factorial(3))
```

该程序的输出是：

```
Factorial of 2: 2
Factorial of 3: 6
```