title: UIView
date: 2017-08-04 09:52:41
tags: [ui, ios]
categories: ios入门
---

创建UIView,在一个视图上增加一个视图
<!-- more -->

``` c
//
//  ViewController.m
//  2UIView
//
//  Created by wanggang on 2017/8/4.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "ViewController.h"

@interface ViewController ()

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    // Do any additional setup after loading the view, typically from a nib.
    // x,y,width,height
    UIView *subview = [[UIView alloc] initWithFrame:CGRectMake(100, 100, 100, 50)];
    subview.backgroundColor = [UIColor blueColor];
    [self.view addSubview:subview];
}


- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}


@end

```
