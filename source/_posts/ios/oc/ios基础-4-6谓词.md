---
title: ios基础-4-6谓词
date: 2017-07-28 9:04:41
tags: [oc, ios]
categories: ios入门
---

## what

按照一定的条件，对数据进行处理和过滤

## how

### 步骤

- 创建谓词对象
- 设定过滤条件
- 应用到对象上

``` c
//
//  Person.h
//  4-6谓词对象
//
//  Created by wanggang on 2017/7/28.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface Person : NSObject
@property(nonatomic, copy)NSString *name;
@property(nonatomic, assign)int age;
@end

//
//  Person.m
//  4-6谓词对象
//
//  Created by wanggang on 2017/7/28.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person.h"

@implementation Person

@end

//
//  main.m
//  4-6谓词对象
//
//  Created by wanggang on 2017/7/28.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        NSArray *arr1 = [[NSArray alloc] initWithObjects:@"ios", @"pc", @"android", nil];
        // 相等
        NSPredicate *predicate1 = [NSPredicate predicateWithFormat:@"SELF == 'pc'"];
        NSArray *arr2 = [arr1 filteredArrayUsingPredicate:predicate1];
        NSLog(@"%@", arr2);
        // 逻辑判断
        NSArray *arr3 = [[NSArray alloc] initWithObjects:@"22", @"1", @"222", nil];
        NSPredicate *predicate2 = [NSPredicate predicateWithFormat:@"SELF.intValue > 10"];
        NSArray *arr4 = [arr3 filteredArrayUsingPredicate:predicate2];
        NSLog(@"%@", arr4);
        // 关键词
        NSArray *arr5 = [[NSArray alloc] initWithObjects:@"hackingangle", @"wangli", @"zhangwu", nil];
        NSPredicate *predicate3 = [NSPredicate predicateWithFormat:@"SELF CONTAINS %@", @"angle"];
        NSArray *arr6 = [arr5 filteredArrayUsingPredicate:predicate3];
        NSLog(@"%@", arr6);
        // 过滤by对象属性
        Person *person1 = [[Person alloc] init];
        person1.name = @"hackingangle";
        person1.age = 27;
        Person *person2 = [[Person alloc] init];
        person2.name = @"haha";
        person2.age = 17;
        NSMutableArray *users = [[NSMutableArray alloc] init];
        [users addObject:person1];
        [users addObject:person2];
        for (Person *user in users) {
            NSLog(@"%@", user.name);
        }
        NSPredicate *predicate4 = [NSPredicate predicateWithFormat:@"age > %d", 18];
        NSArray *users2 = [users filteredArrayUsingPredicate:predicate4];
        for (Person *user in users2) {
            NSLog(@"filtered = %@", user.name);
        }
        
    }
    return 0;
}

```
