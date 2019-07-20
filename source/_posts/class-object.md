---
title: 类和对象
date: 2018-07-11 00:03:00
categories: [技术]
tags: [scala]
---
## 类、成员、方法

```
class MyFirstClass {
	var x = 0 // 可变成员变量
	val v = 1 // 不可变成员变量
    def addNum(num: Int): Unit = {
        x += num
    }
}
```

这样就可以创建对象：`val m = new MyFirstClass`

这里没有指定访问修饰符，表示默认`public`的。<!--more-->

**注意**：Scala方法参数的一个重要特征是它们都是`val`而非`var`的，这点其实很重要，可以不用考虑是否被重新赋值过。

## 单例对象

Scala比Java更面向对象的一点是Scala不允许有静态成员。针对这样的场景，Scala的解决方案是提供了单例对象（`object`）。

`object`和`class`同名时，互相称为伴生对象和伴生类。而这两者必须定义在同一个源码文件中，且他们之间可以访问互相的私有成员。

Scala为每个源码文件，都隐式地引入了`java.lang`和`scala`包的成员，以及名为`Predef`的单例对象中的所有成员，建议读者可以大致阅读下`Predef`中内容。