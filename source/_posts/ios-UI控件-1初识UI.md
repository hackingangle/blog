---
title: ios-UI控件-1初识UI
date: 2017-08-03 08:53:41
tags: [ios, ui]
---

<!-- -->
# UI

在IOS开发中，开发者需要具备一种render界面能力，这里我们介绍界面元素，如果通过代码来控制界面元素的呈现

有了上述的能力，可轻松开发出UI,UE要求的页面

## 知识点

- CGRect
    - CGPoint
        - CGFloat
    - CGSize
        - CGFloat
- 坐标系
    - 以左上角为0,0原点，右侧为width，下侧为height
- 窗口承载对象顺序
    - UIWidow
    - ViewController
    - UI组件
        - UIButton
        - UILabel
        - ...
- UIScreen获取设备物理宽高
    - [UIScreen mainScreen].bounds.size.width
    - [UIScreen mainScreen].bounds.size.height
    - [UIScreen mainScreen].bounds.origin.x
    - [UIScreen mainScreen].bounds.origin.y

其中上述`CGRect`,`CGPoint`,`CGSize`都是结构体，`CGFloat`就是float

``` c
struct CGRect {
    CGPoint origin;
    CGSize size;
};
typedef struct CGRect CGRect;
```
``` c
struct CGPoint {
    CGFloat x;
    CGFloat y;
};
typedef struct CGPoint CGPoint;
```
``` c
struct CGSize {
    CGFloat width;
    CGFloat height;
};
typedef struct CGSize CGSize;
```

## how

``` c
//
//  ViewController.m
//  1背景颜色修改
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "ViewController.h"

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    // 通过当前属性view来调整view背景颜色
    self.view.backgroundColor = [UIColor purpleColor];
    // 获取设备大小
    CGFloat x = [UIScreen mainScreen].bounds.origin.x;
    CGFloat y = [UIScreen mainScreen].bounds.origin.y;
    CGFloat width = [UIScreen mainScreen].bounds.size.width;
    CGFloat height = [UIScreen mainScreen].bounds.size.height;
    NSLog(@"%f %f %f %f", x, y, width, height);
}


- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}


@end

```

output
``` shell
2017-08-03 09:03:20.914 1背景颜色修改[24802:2262994] 0.000000 0.000000 414.000000 736.000000

```