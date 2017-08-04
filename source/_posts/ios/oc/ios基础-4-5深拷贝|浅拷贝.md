---
title: ios基础-4-5深拷贝|浅拷贝
date: 2017-07-28 8:32:41
tags: [oc, ios]
categories: ios入门
---

## 概念

对象拷贝分为：

1. 浅拷贝
1. 深拷贝

## 浅拷贝

- 两个对象指向一致的内存空间
- 只是copy了指针
- 对象结束的时候，会调用destruct方法，这种情况下destruct会被调用2次，造成crash
- 数据不独立
- 释放对象不容易

## 深拷贝

- 重新堆中重新申请内存空间，两个对象分别维护自己的内存空间
- 比较容易释放对象

``` c
//
//  Person.h
//  4-5深拷贝浅拷贝
//
//  Created by wanggang on 2017/7/28.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Cocoa/Cocoa.h>

@interface Person : NSObject
@property(nonatomic, copy)NSString *name;
@end


//
//  Person.m
//  4-5深拷贝浅拷贝
//
//  Created by wanggang on 2017/7/28.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person.h"

@implementation Person
-(id)copyWithZone:(NSZone*)zone{
    Person *copy = [[[self class]allocWithZone:zone]init];
    copy.name = [self.name copyWithZone:zone];
    return copy;
}
@end

//
//  main.m
//  4-5深拷贝浅拷贝
//
//  Created by wanggang on 2017/7/28.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Person *person1 = [[Person alloc] init];
        person1.name = @"hackingangle";
        Person *person2 = [person1 copy];
        NSLog(@"%@", person2.name);
        NSLog(@"%@ %@", person1, person2);
        
        // 可变地址空间<->不可变地址空间
        NSString *str1 = @"hackingangle";
        // 浅拷贝
        NSMutableString *str2 = [str1 copy];
        // 深拷贝
        NSMutableString *str3 = [str1 mutableCopy];
        NSLog(@"%@ %@ %@", str1, str2, str3);
        NSLog(@"%p %p %p", str1, str2, str3);
    }
    return 0;
}

```
