title: ios2
date: 2016-10-03 11:57:09
tags: ios
---

我们在ios1基础之上再增强我们的功能，每一次在刷新label显示的同时，完成的是一个数字的录入操作，如果保存录入的数字呢？

我们在swift当中存在各种数据类型，如：数组，字典，集合等，本次我们使用数组就可以完成数据录入后的保存工作，是不是很赞👍！

好吧，先来介绍下数组的简单使用：
声明、初始化、操作
- 声明、初始化：
    - 新
        - `var arr:[String] = []`
        - `var arr = [String]()`
    - 老
        - `var arr:Array<String> = []`
        - `var arr = Array<String>()`
- 操作
    - 新增
        - `arr.append("hello")`

在知道了如何使用数组来缓存数据之后，我们再来学习一个新的概念。
这个概念有点像是JAVA中的setter和getter，也类比与php中的魔术方法__setter和__getter。
在swift中，我们称之为：可计算属性（`computed property`），使用方法如下：
```
var demoValue:String {
    set: {

    }

    get: {
        return "Hello,World."
    }
}
```

修改ViewController.swift源码，如下：
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

    var isFirstClick = true

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
        if isFirstClick {
            // 更新标签内容
            display.text = sender.currentTitle!
            isFirstClick = false
        } else {
            // 更新了标签内容
            display.text = display.text! + sender.currentTitle!
        }
    }

    
    @IBAction func clear() {
        display.text = "0"
        isFirstClick = true
        numStack = []
    }
    
    // v1.1新增：1>计算属性，自动格式化字符串为双精度浮点型；2>设置数据到双精度数组中
    // 计算属性，格式化字符串为双精度浮点型
    var displayValue:Double {
        get {
            let fNum:Double = NumberFormatter().number(from: display.text!)!.doubleValue
            print("computed property get called. value is \(fNum)")
            return fNum
        }
        set {}
    }
    
    // 初始化浮点型数组
    var numStack = Array<Double>()

    // 新增加入按钮，把输入的数字缓存起来
    @IBAction func enter() {
        // 存储数据到数组
        numStack.append(displayValue)
        // 重新计数
        isFirstClick = true
        // 输出当前数组
        print("numStack is \(numStack)")
    }
}

```
