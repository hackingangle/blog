title: swift 10
date: 2016-09-28 08:18:04
tags: swift
---

如何更好的去操作字符串，我们在编写程序的过程中，必须要学习的内容。

字符串常见的操作有：拼接字符串，字符串比较，字符串前缀后缀匹配等

字符串比较要注意：比较的是从第一位开始的asc码表的位置，`A<a`,`a<b`，不比较长度。

``` swift

let msg = "Hello,"
let name = "hackingangle"
// 字符串的拼接
let welcome = msg + name

// 比较字符串大小
let str1 = "abc"
let str2 = "abc"
// true
str1 == str2
let str3 = "abd"
// true
str1 < str3
let str4 = "abcd"
// true
str3 > str4


let names = [
    "hackingangle",
    "john smith",
    "lili smith",
    "lili zhang",
]
// 直接输出字符串
for name in names {
    print("\(name)")
}
// 匹配字符串前缀或者后缀
var countPrefix = 0
var countSubffix = 0
for name in names {
    if name.hasPrefix("lili") {
        // swift3 废弃了 count++语法
        countPrefix += 1
    }
    if name.hasSuffix("smith") {
        countSubffix += 1
    }
}

```
