title: swift 09
date: 2016-09-27 23:48:52
tags: swift
---

今天我们来介绍下字符串，字符串是每一个编程语言中最常用的类型，请允许我这么介绍今天的主人公－字符串！

来一起看看如何使用他吧！

字符串分为变量字符串和常量字符串，常量字符串不可以进行覆盖值操作和拼接操作。

初始化空字符串可以使用`""`或`String()`。

今天在使用过程中发现，swift的新老版本api差异很大，很多方法都被废弃了，如：countElements等。

``` swift
//: Playground - noun: a place where people can play
import UIKit
// 常量和变量区别
let str = "hello,"
// hackingangle💖
var name = "hackingangle\u{1F496}"
// 提示：Left side of mutating operator isn`t mutable: 'str' is a 'let' constant
// str += "guest"
name = str + name

// 初始化空字符串
var str2 = ""
var str3 = String()
// Init
str2 += "Init"
// false
str2.isEmpty
// true
str3.isEmpty

// 使用for...in循环字符串
// 提示：Type 'String' does not conform to protocol 'Sequence'
//for c in str2 {
//    println(c)
//}

// 拼字符
var ch:Character = "!"
str2.append(ch)
str2
```
