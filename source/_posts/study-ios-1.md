title: ios1
date: 2016-10-03 10:10:37
tags: ios
---

## 概述

ios开发除了解决语言(oc,swift)的学习之外，还要了解apple对于ios的N多封装的使用。
ios利用了`MVC`模型来搭建用户交互行为。
例如本讲所讲，利用`storyboard`来快速搭建一个UI界面；为UI界面上的按钮来添加点击事件，并且对点击事件的行为进行更新，从而控制视图的显示。
视图的定位在默认的`Main.storyboard`文件中，利用xcode提供的工具就可以搭建了。

点击事件行为的定义在`ViewController.swift`中。代码如下：

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
    }
}


```

## 总结

1. 按钮上可以添加事件
    - 事件可为多个
    - 每个事件都依次会被触发
1. Controller类的成员变量在App的整个存活周期内都会持有，不会被触发
1. 成员变量使用，比较php而言，不用`this.`关键词，直接使用即可
1. "ALT"按键配合鼠标点击，可以知道Swift类型推断的结果(在不指定类型的情况下)
1. "CONTROL"按键配合鼠标点击，可以查看API简介
1. UIButton
    - currentTitle(String?)
1. UILabel
    - text(String?)
1. Value Binding技术可以来结包Optional类型
    - demo如下：
    ``` swift
        var username:String?= "hackingangle"
        if let username = username 
        {
            print("username = \(username)")
        } else {
            print("invalid username")
        }
    ```
