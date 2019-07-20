---
title: 面向对象基础
date: 2018-07-14 19:13:54
categories: [技术]
tags: [scala]
---
## 类定义

我们知道所有的有理数，都可以表示为一个分数，形如n/d，下面就要构造一个有理数类。

`class Rational(n: Int, d: Int)`

简单的一行代码，即定义了类以及构造方法。但是需要注意这里的`n`和`d`是类参数，而非成员变量，因此如下调用会报错：<!--more-->

```scala
scala> a.d
<console>:13: error: value d is not a member of Rational
```

当然，如果想变成成员变量很简单，在前面加个变量声明的符号`val`或者`var`即可：

`class Rational(var n: Int, val d: Int)`

接下来可以直接对成员进行赋值和使用：

```scala
scala> a.n = 10
a.n: Int = 10
scala> a.n
res7: Int = 10
```

## getter和setter方法

可以利用注解简单地生成getter和setter方法：

````scala
import scala.beans.BeanProperty
class Rational(@BeanProperty var n: Int, var d: Int)
````

实际开发中，可能遇到setter方法中有逻辑判断的，那么这种方法就不能采用。而且如果不是为了兼容Java的写法的话，建议单独使用Scala中方式如下：

```scala
class Person (private var _name: String) {
  def name = _name
  def name_= (newName: String) = {
    _name = newName
  }
}
```

注意这里的 `name_=`是一个整体，是方法名，会被编译为 `name_$eq` ，因为JVM不允许方法名中出现“=”符号。

下面总结出2点：

1. 使用private var关键字修饰构造方法参数，一般在参数名前加上下划线“_”。
2. 定义getter和setter方法：getter方法使用“参数名”命名；setter方法使用“参数名_="来命名，“=”之前不能有空格。

## 辅助构造方法

即为类定义多个构造方法，类似于Java中的构造方法重构：

```scala
class Person (private var _name: String) {
  def name = _name
  def name_=(newName: String) = {
    _name = newName
  }

  def this() {
    this("default")
    println("new default")
  }
}
```

这里需要注意的是辅助构造方法，必须首先调用同一个类的另一个构造方法。

