---
title: ios基础-5-1类间通信block
date: 2017-08-02 07:00:41
tags: [oc, ios]
categories: ios入门
---

## block
1. block就是一个代码块
1. block本质上就是一个函数指针，即代码块的内存地址
1. block常用来传递值，本质就是把block地址传递到调用block地方
1. block可以访问外部变量，但是不能修改外部变量
1. 如果一定要修改外部变量，可以在外部变量前面增加修饰词`_block`
1. 利用typedef可以定义block类型
    - `typedef int (^sum) (int, int)`

## 类通信坑-交叉引用

block极力避免的就是类之间的方法的互相依赖，也就是交叉引用

交叉引用来个定义：双方类的对象互相持有对方，导致对象不被释放

``` c
//
//  main.m
//  5-1类通信block
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "AClass.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        AClass *classa = [[AClass alloc] init];
        [classa testBlock];
    }
    return 0;
}

//
//  AClass.h
//  5-1类通信block
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "BClass.h"

@interface AClass : NSObject
-(void)testBlock;
@end

//
//  AClass.m
//  5-1类通信block
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "AClass.h"

@implementation AClass
-(void)testBlock {
    NSLog(@"testBlock from AClass");
    BClass *classb = [[BClass alloc] init];
    [classb testB];
}
@end

//
//  BClass.h
//  5-1类通信block
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "AClass.h"

@interface BClass : NSObject
-(void)testB;
@end

//
//  BClass.m
//  5-1类通信block
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "BClass.h"

@implementation BClass
-(void)testB {
    NSLog(@"testB from BClass");
    AClass *classa = [[AClass alloc] init];
    [classa testBlock];
}
@end

```

## block解决类之间相互通信

block解决通信中的交叉引用问题

1. Block声明在被调用类中

``` c
//
//  main.m
//  5-1类通信block
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "AClass.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        AClass *classa = [[AClass alloc] init];
        [classa testBlock];
    }
    return 0;
}

//
//  AClass.h
//  5-1类通信block
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "BClass.h"

@interface AClass : NSObject
-(void)testBlock;
@end

//
//  AClass.m
//  5-1类通信block
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "AClass.h"

typedef void (^MyBlock) (int);
@implementation AClass
-(void)testBlock {
    NSLog(@"testBlock from AClass");
    BClass *classb = [[BClass alloc] init];
//    MessageOfClass message = ^(NSString* msg) {
//        NSLog(@"I am defined in Aclass, This outside message: %@", msg);
//    };
//    [classb testB:message caller:@"classa"];
    [classb testB: ^(NSString* msg) {
        NSLog(@"I am defined in Aclass, This outside message: %@", msg);
    } caller:@"classa"];
}
@end

//
//  BClass.h
//  5-1类通信block
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
typedef void (^MessageOfClass) (NSString*);
@interface BClass : NSObject
-(void)testB:(MessageOfClass) message caller:(NSString*)caller;
@end

//
//  BClass.m
//  5-1类通信block
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "BClass.h"

@implementation BClass
-(void)testB:(MessageOfClass) message caller:(NSString*)caller {
    NSLog(@"caller : %@", caller);
    NSLog(@"testB from BClass");
    message(@"Message From BClass");
}
@end

```

output

``` shell
2017-08-02 08:16:43.178927+0800 5-1类通信block[18356:1455053] testBlock from AClass
2017-08-02 08:16:43.179119+0800 5-1类通信block[18356:1455053] caller : classa
2017-08-02 08:16:43.179140+0800 5-1类通信block[18356:1455053] testB from BClass
2017-08-02 08:16:43.179165+0800 5-1类通信block[18356:1455053] I am defined in Aclass, This outside message: Message From BClass
Program ended with exit code: 0
```

