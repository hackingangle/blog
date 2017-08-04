---
title: ios基础-3-5分类|类扩展
date: 2017-07-26 23:15:41
tags: [oc, ios]
categories: ios入门
---

## 分类
分类：类的扩展，不知道原来类的情况下，新增方法，但是`不可以添加属性`

!!! 方法名相同，分类中方法优先级高，会覆盖之前的方法

``` c
//
//  main.m
//  3-5分类|扩展
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"
// 分类
#import "Person+Student.h"
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Person *person = [[Person alloc] init];
        [person eating];
        // 分类
        [person learning];
    }
    return 0;
}

//
//  Person.h
//  3-5分类|扩展
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface Person : NSObject
-(void)eating;
@end


//
//  Person.m
//  3-5分类|扩展
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person.h"

@implementation Person
-(void)eating{
    NSLog(@"person eating...");
}
@end

//
//  Person+Student.h
//  3-5分类|扩展
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person.h"

@interface Person (Student)
-(void)learning;
@end

//
//  Person+Student.m
//  3-5分类|扩展
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person+Student.h"

@implementation Person (Student)
-(void)learning{
    NSLog(@"learning is person what person");
}
@end
```

## 类扩展
想扩展属性，必须用类扩展

同一个.h文件中只能定义一个`@interface`标签，所以类扩展我们必须放到.m文件中

``` c
//
//  main.m
//  3-5分类|扩展
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"
// 分类
#import "Person+Student.h"
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Person *person = [[Person alloc] init];
        [person eating];
        // 分类
        [person learning];
    }
    return 0;
}

//
//  Person.h
//  3-5分类|扩展
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface Person : NSObject
-(void)eating;
@end


//
//  Person.m
//  3-5分类|扩展
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person.h"

@interface Person() {
    BOOL male;
}
@end

@implementation Person
-(id)init{
    self = [super init];
    if (self) {
        male = true;
    }
    return self;
}
-(void)eating{
    if (male) {
        NSLog(@"male");
    } else {
        NSLog(@"female");
    }
    NSLog(@"person eating...");
}
@end

//
//  Person+Student.h
//  3-5分类|扩展
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person.h"

@interface Person (Student)
-(void)learning;
@end

//
//  Person+Student.m
//  3-5分类|扩展
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person+Student.h"

@implementation Person (Student)
-(void)learning{
    NSLog(@"learning is person what person");
}

@end


```