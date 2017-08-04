title: swift 08
date: 2016-09-27 23:26:51
tags: swift
---

swift中逻辑运算符比较简单，分为三种：`&&`，`||`，`!`

另外，要与之区分的是位运算符，分为两种：`&`，`|`。

``` swift
// 位运算符
// 00000001
let a = 1
// 00000110
let b = 6
// 0

let c = a & b

// 7
let d = a | b

// 逻辑运算符
let bol1 = true
let bol2 = false

bol1 && bol2
bol1 || bol2
!bol1
!bol2
```
