title: swift 02
date: 2016-09-22 09:10:19
tags: swift
---

选择语句`if`，真-true，假-false

``` swift
let flag = true
let flagB = false

if flag {
    print("flag true")
}

let check = 1+1 == 2

if flagB {
    print("flag b")
} else if check {
    print("1+1=2")
} else {
    print("defalut")
}


var val1:Int = 1
// swift中强类型，整型不可以当作Bool类型，异常提示：'Int' is not convertible to 'Bool'
if val1 {
    print("check val int as logical")
}
```
