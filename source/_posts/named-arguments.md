---
title: Scala之旅——带名参数
date: 2018-10-28 19:45:00
categories: [翻译]
tags: [scala]
description: 原文：https://docs.scala-lang.org/tour/named-arguments.html
---

调用方法时，可以使用参数名给参数带上标签，正如：

```tut
def printName(first: String, last: String): Unit = {
  println(first + " " + last)
}

printName("John", "Smith")  // Prints "John Smith"
printName(first = "John", last = "Smith")  // Prints "John Smith"
printName(last = "Smith", first = "John")  // Prints "John Smith"
```

注意，带名参数的顺序可以重新调整。然而，如果有些参数是带名的，有些则不是，那么不带名的参数必须在最前面，并且是按照方法定义中参数的顺序出现。

```tut:fail
printName(last = "Smith", "john") // error: positional after named argument
```

注意带名参数不适用于调用Java方法。<!--more-->