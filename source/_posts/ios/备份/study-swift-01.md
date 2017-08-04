title: swift 01
date: 2016-09-21 08:33:51
tags: swift
---

# 任务
## 目标

成为`full stack engineer`.

## 详情

- swift基础语法

# swift基础语法

开始学习一门语言的语法，无外乎从`数据类型`开始，常见的类型：整型、浮点型、字符串。

从语言本身区分：`强类型`语言和`弱类型`语言，强类型语言通常要求我们在使用变量之前去声明变量的类型。

变量本身分为两种：一种能够修改值，一种不能够修改值，能修改的叫`变量`；不能修改的叫`常量`。

``` swift
//: Playground - noun: a place where people can play

// 声明常量
let maxNum = 1000

// 声明变量
var userName = "hackingangle"

// 声明多个变量
var x = 0.0, y = 0.0, z = 0.0

// 强类型语言，如何来声明类型呢？看下面的演示
var d1, d2, d3 : Double;

// 赋值给一个常量值会报错:Cannot assign to vale:'maxNum' is 'let' constant
// maxNum = 100

// 变量可以被修改值
userName = "root"

// swift是类型安全的，赋值给一个初始化为字符串的变量整型，会报错：Cannot assign value of type 'int' to type 'String'
// userName = 1000

// 整型演示
let decimalInt:Int = 17
let binaryInt:Int = 0b10001
let octalInt:Int = 0o21
let hexadecimalInt:Int = 0x11

// 浮点型
let floatA = 0.012
let floatB = 1.2e-2

// 强制类型转换
let changeFloatA:Float = 1
// 由于丢失了精度，会报错：Cannot convert value of type 'Double' to specified type 'Int'
// let changeIntA:Int = 1.2
// 可以强制类型转化（在开发者明确的情况下丢掉一些精度）
let changeIntB:Int = Int(1.2)

let addA:Int = 3
let addB:Double = 0.1415926
// 由于精度不一样，会有报警：Binary operator '+' cannot be applied to operands of type 'Double' and 'Int'
let sumA:Double = addB + Double(addA)

// 整型
let intA = 1000000
// 更人性化的表示法
let intB = 1000_000
let intC = 10_0000

// 变量可以使用中文，全面支持unicode字符
var 姓名 = "hackingangle"
var 欢迎语 = "您好"
var 一句话 = 欢迎语 + "，" + 姓名

```
