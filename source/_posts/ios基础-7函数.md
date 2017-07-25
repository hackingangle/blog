---
title: ios基础-7-函数
date: 2017-07-25 23:08:41
tags: ios
---

函数

如果不在main前面声明、创建，需要声明签名，如:hello()

``` c
//
//  main.m
//  7函数
//
//  Created by wanggang on 2017/7/25.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
void hello();
int calculate(int a, int b, char c) {
    int ret = 0;
    switch(c) {
        case '+':
            ret = a + b;
            break;
        case '-':
            ret = a - b;
            break;
        case '*':
            ret = a * b;
            break;
        case '/':
            ret = a / b;
            break;
        default:
            break;
    }
    return ret;
}
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        hello();
        NSLog(@"%d", calculate(2, 3, '+'));
        NSLog(@"%d", calculate(2, 3, '-'));
        NSLog(@"%d", calculate(2, 3, '*'));
        NSLog(@"%d", calculate(2, 3, '/'));
    }
    return 0;
}

void hello() {
    NSLog(@"hello world");
}
```
