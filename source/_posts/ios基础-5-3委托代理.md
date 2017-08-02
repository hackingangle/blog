---
title: ios基础-5-3委托代理
date: 2017-08-02 09:05:41
tags: ios
---

## what

一种设计模式

- 生成协议
    - 交钱给我
- 委托
    - 我把我的工具包给你
- 代理方法
    - 拿着我给你的工具包工作
    
## how

- optional声明可选实现，不黄色提示

实现了一个boss委托employee去代理老板工作

``` c
//
//  main.m
//  5-3类通信委托代理
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Boss.h"
#import "Employee.h"
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Boss *boss = [[Boss alloc] init];
        Employee *employee = [[Employee alloc] initWithBoss:boss];
        [boss work];
    }
    return 0;
}

//
//  Boss.h
//  5-3类通信委托代理
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
@protocol WorkDelegate;

@interface Boss : NSObject
@property(nonatomic, weak) id<WorkDelegate> Delegate;
- (void) work;
@end

@protocol WorkDelegate <NSObject>
@optional
- (void) work;
@end

//
//  Boss.m
//  5-3类通信委托代理
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Boss.h"

@implementation Boss
- (void) work {
    if ([self.Delegate respondsToSelector:@selector(work)]) {
        [self.Delegate work];
    } else {
        NSLog(@"委托的代理不存在");
    }
}
@end

//
//  Employee.h
//  5-3类通信委托代理
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Boss.h"

@interface Employee : NSObject<WorkDelegate>

- (id) initWithBoss:(Boss *)boss;
@end


//
//  Employee.m
//  5-3类通信委托代理
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Employee.h"

@implementation Employee
- (id) initWithBoss:(Boss *)boss {
    self = [super init];
    if (self) {
        boss.Delegate = self;
    }
    return self;
}

- (void) work {
    NSLog(@"我是雇员，我为我的老板效力！");
}
@end

```

output

``` shell
2017-08-02 09:42:41.718044+0800 5-3类通信委托代理[18849:1539086] 我是雇员，我为我的老板效力！
Program ended with exit code: 0
```