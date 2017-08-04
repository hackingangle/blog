---
title: ios基础-6-循环
date: 2017-07-25 22:38:41
tags: [oc, ios]
categories: ios入门
---

循环

- while (XX)
- do while
    - 至少执行一次，即使条件不满足，很少使用
- for
    - 最重要的循环
    - 快速枚举
- break|continue
    - break
        - 跳出所有循环
    - continue
        - 跳出本次循环
         
``` c
//
//  main.m
//  6循环
//
//  Created by wanggang on 2017/7/25.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        int count = 2;
        while (count > 0) {
            count--;
            NSLog(@"hi in while");
        }
        // 至少执行一次
        do {
            count--;
            NSLog(@"hi in do while");
        } while (count >0);
        // 最重要的循环
        for (int count = 2; count > 0; count--) {
            if (count == 2) {
                NSLog(@"break all for");
                break;
            }
            NSLog(@"hi in for");
        }
    }
    return 0;
}

```