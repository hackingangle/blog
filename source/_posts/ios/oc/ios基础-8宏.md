---
title: ios基础-8-宏
date: 2017-07-25 23:17:41
tags: [oc, ios]
categories: ios入门
---

## 宏

- 定义变量
- 定义运算
- 定义方法

## 编译参数

- if
- endif
- ifdef
- TARGET_OS_MAC

``` c
//
//  main.m
//  8宏定义编译
//
//  Created by wanggang on 2017/7/25.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#define PI 3.141592
#define SUM(x,y) x+y
#define MYLOG(x) NSLog(@"My log is %f", x)
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        MYLOG(PI);
        MYLOG(SUM(1.1,2));
        #if TARGET_OS_MAC
            NSLog(@"mac");
        #endif
        
        #define DEBUG_MODE 0
        #if DEBUG_MODE
            NSLog(@"debug");
        #else
            NSLog(@"no debug");
        #endif
        
        #ifdef DEBUG_MODE
            NSLog(@"define debug");
        #endif
        
        // 系统定义的debug模式
        #ifdef DEBUG
            NSLog(@"系统定义debug模式");
        #endif
    }
    return 0;
}

```