---
title: 定时器
date: 2017-08-02 15:51:41
tags: [oc, ios]
categories: ios入门
---
定时执行某个方法，可以带参数，可以选择循环；NSRunLoop；统计程序耗时 
<!-- more -->
1. 两种定时器的创建方法
1. 时间精确不到ms级别
1. 程序计算执行耗时的方法
1. 定时器创建可以传递参数

``` c
//
//  Timer.m
//  5-4定时器
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Timer.h"
@interface Timer() {
    NSTimer *timer1;
    NSTimer *timer2;
}
@end

@implementation Timer
-(void)startTimer {
    // 第一种计时器
//    timer = [NSTimer timerWithTimeInterval:1 target:self selector:@selector(callbackTimer) userInfo:nil repeats:true];
//    [[NSRunLoop currentRunLoop] addTimer:timer forMode:NSDefaultRunLoopMode];
//    [[NSRunLoop currentRunLoop] run];
    // 第二种计时器
    // 携带参数
//    timer2 = [NSTimer scheduledTimerWithTimeInterval:1 target:self selector:@selector(callbackTimer2:) userInfo:@"hackingangle" repeats:false];
//    [timer2 fire];
//    // 时间精度毫秒
//    timer1 = [NSTimer timerWithTimeInterval:0.01 target:self selector:@selector(callbackTimer) userInfo:nil repeats:false];
//    [[NSRunLoop currentRunLoop] addTimer:timer1 forMode:NSDefaultRunLoopMode];
//    [[NSRunLoop currentRunLoop] run];
    // 打印耗时
    NSDate *timeMark = [NSDate date];
    // 统计程序执行开始
    for (int i=0; i<10000; i++) {
        NSLog(@"HelloWorld");
    }
    // 统计程序执行结束
    NSTimeInterval timeInterval = [timeMark timeIntervalSinceNow];
    float timeNow = 0 - timeInterval;
    NSLog(@"%@ %f", timeMark, timeNow);
    
}
-(void)callbackTimer{
    NSLog(@"timer callback");
}
-(void)callbackTimer2:(NSTimer*) timer{
    NSLog(@"timer callback2 %@", timer.userInfo);
}
@end

```