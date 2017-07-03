title: ios3
date: 2016-10-09 22:20:40
tags: ios
---

今天计算器的小程序完成了，总结一下！

知识点：

- storyboard使用
    - 为UILabel控件添加置顶的约束
    - 部分网格的动态展开视图
- ViewController控制逻辑编写
    - 为了功能模块的复用->抽象为方法
    - 匿名方法使用（可以缩略写，利用了类型推断的强大语言特性）

``` swift
//
//  ViewController.swift
//  Cracker
//
//  Created by hackingangle on 16/10/2.
//  Copyright © 2016年 hackingangle. All rights reserved.
//

import UIKit

class ViewController: UIViewController {

    // 定义了当前视图下的label标签的封装对象
    // 可以通过在行为中调用此对象来更新对应的标签内容(label.text)
    @IBOutlet weak var display: UILabel!

//    var isFirstClick = true
    var userIsTheMiddleOfTypingANumber = false

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }

    // 接收了点击按钮发送的事件，在这里定义了事件触发时候的行为
    // sender是点击按钮的封装，可以通过sender.currentTitle来获取按钮显示的内容
    @IBAction func sendNum(_ sender: UIButton) {
        let digit:String = sender.currentTitle!
        if userIsTheMiddleOfTypingANumber {
            // 更新了标签内容
            display.text = display.text! + digit
        } else {
            // 更新标签内容
            display.text = digit
            userIsTheMiddleOfTypingANumber = true
        }
    }

    
    @IBAction func clear() {
        display.text = "0"
        userIsTheMiddleOfTypingANumber = false
        numStack = []
    }
    
    // v1.1新增：1>计算属性，自动格式化字符串为双精度浮点型；2>设置数据到双精度数组中
    // 计算属性，格式化字符串为双精度浮点型
    var displayValue:Double {
        get {
            let fNum:Double = NumberFormatter().number(from: display.text!)!.doubleValue
            print("get called")
            return fNum
        }
        set {
            print("set called, newValue = \(newValue)")
            // 更新label内容
            display.text = "\(newValue)"
            // 重新计数
            userIsTheMiddleOfTypingANumber = false
        }
    }
    
    // 初始化浮点型数组
    var numStack = Array<Double>()

    @IBAction func enter() {
        // 存储数据到数组
        numStack.append(displayValue)
        // 重新计数
        userIsTheMiddleOfTypingANumber = false
        // 输出当前数组
        print("numStack is \(numStack)")
    }

    // +-*/的操作符
    @IBAction func operate(_ sender: UIButton) {
        if userIsTheMiddleOfTypingANumber {
            enter()
        }
        if let btnValue = sender.currentTitle {
            switch btnValue {
            case "+":
//                performOperation(operation: inc)
                // 匿名：最初
                performOperation(operation: {(opt1:Double, opt2:Double) in return opt1 + opt2})
            case "-":
//                performOperation(operation: dec)
                // 匿名：通过类型推断，预知参数类型，返回值表达式
                performOperation(operation: {(opt1, opt2) in opt1 - opt2})
            case "*":
//                performOperation(operation: multiply)
                // 匿名函数位于调用参数最后一个，可以外置
                performOperation(operation: ) {$0 * $1}
            case "/":
//                performOperation(operation: devide)
//                performOperation {$0 / $1}
                // 如果参数只有一个匿名函数，则可以直接不带参数
                performOperation {$0 / $1}
            default:
                break
            }
        } else {
            print("click invalid button, newValue")
        }
    }
    
    // 加减乘除基础方法
    func multiply(opA:Double, opB:Double) -> Double {
        return opA * opB
    }
    func devide(opA:Double, opB:Double) -> Double {
        return opA / opB
    }
    func inc(opA:Double, opB:Double) -> Double {
        return opA + opB
    }
    func dec(opA:Double, opB:Double) -> Double {
        return opA - opB
    }
    
    // 参数声明了方法
    func performOperation(operation: (Double, Double) -> Double) {
        if numStack.count >= 2 {
            displayValue = operation(numStack.removeLast(), numStack.removeLast())
            enter()
        }
    }

    // 参数声明了方法, swift3中貌似已经废除
//    func performOperation(operation: (Double) -> Double) {
//        if numStack.count >= 1 {
//            displayValue = operation(numStack.removeLast())
//            enter()
//        }
//    }
}

```
