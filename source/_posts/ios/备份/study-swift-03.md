title: swift 03
date: 2016-09-23 08:56:11
tags: swift
---

今天介绍的是元组(tuples)，如何去理解？

我们先来考虑这样一个问题，如果在`方法`中返回`2个以上`的变量？

第一个想起来的恐怕是－>对象

使用对象有问题吗？我们恐怕要`define`好多没有特殊含义的类，仅仅是为了组合多的数据类型返回。

Swift 和 Python 都为我们提供了语法级别的支持－`元组`

下面我们来详细了解下：

``` swift
// 定义一个元组（声明key和不声明key）
// with key
let registrationResult = (isRegistration:true, nickName: "hackingangle", gender: "man")
// not with key
let connectionResult = (404, "not found")
// 解开没有key的元组
let (connectionStatus, connectionMsg) = connectionResult
connectionStatus
connectionMsg

connectionResult.0
connectionResult.1

// 使用_忽略部分
let (connectionStatus2, _) = connectionResult
connectionStatus2

if connectionStatus == 404 {
    print("return status 404")
}
```
