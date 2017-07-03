title: swift 11
date: 2016-09-28 08:35:41
tags: swift
---

如何去继承使用原来OC中对于字符串操作的方法？

我们只需要引入`Foundation`就可以了。

我们来介绍下字符串的驼峰化，大写，小写，trim，split，join这些常用的方法。

``` swift
var str = "hi, hackingangle"

// 驼峰化
str.capitalized
// 大写
str.uppercased()
str.lowercased()

// trim
var str2 = "   !!!hi!!  "
str2.trimmingCharacters(in: CharacterSet.whitespaces)
str2.trimmingCharacters(in: CharacterSet.init(charactersIn: " !"))

// split
var str3 = "welcome to the swift world."
str3.components(separatedBy: " ")

var str4 = "Welcome to the swift world.NOW-LET-US-START!"
str4.components(separatedBy: CharacterSet.init(charactersIn: " -"))

// join[swift3被废弃了]
var str5 = "-"

```
