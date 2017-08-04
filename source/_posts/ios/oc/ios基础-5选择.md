---
title: ios基础-5-选择
date: 2017-07-24 23:49:41
tags: [oc, ios]
categories: ios入门
---

- if
    - if else
    - ... else if ...
- switch
    - 跟C用法相同

``` c
//
//  main.m
//  5选择if
//
//  Created by wanggang on 2017/7/24.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        int a = 10;
        // if
        if (a > 1) {
            NSLog(@"a > 1");
        }
        // if-else
        if (a > 10) {
            NSLog(@"a > 10");
        } else {
            NSLog(@"a < 10");
        }
        // 嵌套
        if (a != 0) {
            if (a > 0) {
                NSLog(@"a > 0");
            } else {
                NSLog(@"a < 0");
            }
        }
        // if-else if-else
        if (a < 0) {
            NSLog(@"a < 0");
        } else if (a > 0) {
            NSLog(@"a > 0");
        } else {
            NSLog(@"a = 0");
        }
        // break必不可少
        int flag = 2;
        switch (flag) {
            case 10:
                NSLog(@"10");
                break;
            case 2:
                NSLog(@"2");
                break;
            default:
                NSLog(@"default");
                break;
        }
    }
    return 0;
}

```
