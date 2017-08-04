title: swift 12
date: 2016-09-28 08:54:27
tags: swift
---

swift中的数组作为强类型，是不允许初始化的时候直接不指定类型的空数组创建的。

因为一切变量，都要求我们必须知道他们的类型，如果声明了一个没有初始化的没有指定类型的空数组，编译器也不知道应该指定什么类型。

``` swift

// array and dictionary

var arr = ["hackingangle", "john"]
var arr2:[String] = ["a", "b"]
var arr3:Array<String> = ["c", "d", "e", "f"]

// 类型指定后不能修改，否报错：Cannot assign value of tyoe 'Int' to type 'String'
//arr[0] = 1
arr[0] = "Hello"

// 初始化
var arr4:Array<String> = []
arr4.append("a")
var arr5:[String] = []
arr5.append("b")
var arr6 = Array<String>()
arr6.append("c")
var arr7 = [String]()
arr7.append("d")

// 数组合并
let arr8 = arr4 + arr5 + arr6 + arr7
```
