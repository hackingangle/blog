---
title: ios基础-3-2-构造方法&实例方法&类方法
date: 2017-07-26 22:27:41
tags: ios
---

``` c
//
//  main.m
//  3-2构造方法
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
//        Person *person = [[Person alloc] init];
        Person *person = [[Person alloc] initWithName:@"hackingangle" age:27];
        NSLog(@"%@ %d", person.name, person.age);
        // 实例方法
        [person say];
        // 类方法
        [Person work];
    }
    return 0;
}

//
//  Person.h
//  3-2构造方法
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface Person : NSObject
@property(nonatomic, assign) int age;
@property(nonatomic, copy)NSString *name;
-(id) initWithName:(NSString*)name age:(int)age;
-(void) say;
+(void) work;
@end

//
//  Person.m
//  3-2构造方法
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person.h"

@implementation Person
-(id)init {
    self = [super init];
    if (self) {
        self.age = 18;
    }
    return self;
}

-(id)initWithName:(NSString*)name age:(int)age {
    self = [super init];
    if (self) {
        self.name = name;
        self.age = age;
    }
    return self;
}
-(void) say {
    NSLog(@"say");
}
+(void) work {
    NSLog(@"work");
}
@end

```
