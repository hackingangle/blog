title: swift 07
date: 2016-09-27 23:18:08
tags: swift
---

今天让我们来讲讲循环for..in和区间运算符m...n。

区间运算符可以快速初始化一组变量，然后使用for...in将这些变量循环出来。

区间运算符分为两种：1...100与1..<100。第二种使用场景是从0开始的数组输出。

``` swift
// 循环数组
for num in 1...100 {
    print("num = \(num)")
}

// 利用半开区间来循环数组
let names = ["hackingangle", "hackingangle2", "hackingangle3"]
for i in 0..<names.count {
    names[i]
}
```
