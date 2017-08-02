---
title: ios基础-5-2类间通信广播
date: 2017-08-02 08:20:41
tags: ios
---

广播三要素：
- 广播中心
- 发起人
- 接收者

> 广播其实就是一种观察者模式，需要注册观察者，被监听事件触发，通知观察者的回调方法

广播可以支持：
- 1-1
- 1-n
- n-n
- n-1

``` c
//
//  main.m
//  5-2类通信广播
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "ClassA.h"
#import "ClassB.h"
#import "ClassBB.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        ClassA *a = [[ClassA alloc] init];
        ClassB *b = [[ClassB alloc] init];
        ClassBB *bb = [[ClassBB alloc] init];
        [a sendNof];
    }
    return 0;
}

//
//  ClassBB.h
//  5-2类通信广播
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface ClassBB : NSObject

@end

//
//  ClassBB.m
//  5-2类通信广播
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "ClassBB.h"

@implementation ClassBB
- (id) init {
    self = [super init];
    if (self) {
        [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(callbackNof:) name:@"USERINFO" object:nil];
    }
    return self;
}
- (void) callbackNof:(NSNotification*) nof {
    NSLog(@"广播回调 ClassBB");
    NSLog(@"%@ %@", nof.object, [nof.userInfo objectForKey:@"username"]);
}

@end

//
//  ClassA.h
//  5-2类通信广播
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface ClassA : NSObject
- (void) sendNof;
@end

//
//  ClassA.m
//  5-2类通信广播
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "ClassA.h"

@implementation ClassA
- (void) sendNof {
//    [[NSNotificationCenter defaultCenter]postNotificationName:@"USERINFO" object:nil userInfo:nil];
    NSMutableDictionary *dict = [[NSMutableDictionary alloc] init];
    [dict setValue:@"hackingangle" forKey:@"username"];
    [[NSNotificationCenter defaultCenter]postNotificationName:@"USERINFO" object:[NSNumber numberWithInteger:5] userInfo:dict];
}
@end

//
//  ClassB.h
//  5-2类通信广播
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface ClassB : NSObject

@end

//
//  ClassB.m
//  5-2类通信广播
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "ClassB.h"

@implementation ClassB
- (id) init {
    self = [super init];
    if (self) {
        // 参数1:观察者对象是谁
        // 参数2:回调方法是谁，另外selector是什么鬼？？？
        // 参数3:广播名称
        // 参数4:广播附带参数
        [[NSNotificationCenter defaultCenter]addObserver:self selector:@selector(callbackNofication:) name:@"USERINFO" object:nil];
    }
    return self;
}

- (void) callbackNofication {
    NSLog(@"广播回调");
}

- (void) callbackNofication:(NSNotification*)nof {
    NSLog(@"广播回调带参数");
    NSLog(@"%@ %@", nof.object, nof.userInfo);
}

// 销毁观察者
- (void) dealloc {
    [[NSNotificationCenter defaultCenter]removeObserver:self name:@"USERINFO" object:nil];
}
@end

```