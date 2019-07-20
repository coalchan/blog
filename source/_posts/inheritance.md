---
title: 继承关系
date: 2018-07-16 22:10:00
categories: [技术]
tags: [scala]
---

## 继承关系图

![](/images/unified-types-diagram.svg)

<!--more-->

每个类都继承自`Any`类，它有两个子类`AnyVal`和`AnyRef`,`AnyVal`是所有值类的父类，`AnyRef`是所有引用类的父类。

## 底类型（botton types）

在上图中类继承关系的底部，有两个特殊的类`Null`和`Nothing`，它们用于处理一些特殊情况。

`Null`类是`null`引用的类型。

`Nothing`类主要用途是给出非正常终止的信号，例如Scala中内置的`error`方法：

```scala
def error(message: String): Nothing = throw new RuntimeException(message)
```

由于`Nothing`是所有其他类型的子类型，那么该方法可以用来：

```scala
def divide(x: Int, y: Int): Int = 
  if (y != 0) x / y
  else sys.error("cannot divided by zero")
```