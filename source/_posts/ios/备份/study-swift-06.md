title: swift 06
date: 2016-09-27 09:32:48
tags: swift
---

swift 为我们提供了三目的比较运算符`a>b?a:b`，我们可以使用这种语法来进行`Optional`类型的解包。

然而，这就够了吗？

不够，当然不够！swift为我们提供了语法糖，使得我们可以进行快速解包的二目运算符`a??b`。

目前很多第三方类库都已经在使用`??`。

快来`KEAP YOUR CODE CLEAN`！

``` swift
var username:String?

// 表单收集来的数据
username = "hackingangle"

if username != nil {
    print("Hello, \(username!)")
} else {
    print("Hello, Guest")
}

var showUsername0 = username != nil ? username! : "Guest"
print("Hello, \(showUsername0)")

// nil聚合运算符
var showUsername1 = username ?? "Guest"
print("Hello, \(showUsername1)")
```
