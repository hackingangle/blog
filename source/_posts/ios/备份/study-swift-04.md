title: swift 04
date: 2016-09-26 23:16:01
tags: swift
---

## 可选型Optional

swift为什么要在语言级别支撑一个新的语法，可选型？我在刚开始学习`Optional`的时候不禁要问这样的问题。

强类型语言需要对每一个变量都指定一个类型。然后，有的时候，比如，发生了连接异常，初始化失败等等情况的时候，变量会被赋值为空。swift中的空可以用`nil`来表示。所以一个变量可能为一个不为空的XX类的对象，也有可能为空。

重要的地方来了，在强类型语言中，如果一个变量为空时，使用他来直接调用方法或者变量，则会引发一个致命的错误。

所以让开发人员明白，你当前使用的`变量有可能是空的`。－是一件非常非常重要的事情。

swift的非常慷慨的提供了一种语法来告诉开发者－你当前使用的变量有可能为空，强制你来对异常进行捕获处理，强制你来对可能存在的值进行获取（解包）。

通过提供语言级别的特性，来提供开发者程序的健壮性。从而避免严重的，不易发现的，在某些条件下才会被触发的致命错误。

小心Crash！

``` swift
// Optional 声明
var a:Int
// 对于非optional类型，报错：error: variable 'a' used before being initialized
// a
a = 1
a

var b:Int?
b
b = 1
b

// 解包
var username:String? = "hackingangle"
if username != nil {
    print(username)
    print(username!)
} else {
    print("invalid username")
}
```
