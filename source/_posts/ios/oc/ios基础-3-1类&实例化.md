---
title: ios基础-3-1-类实例化
date: 2017-07-26 13:07:41
tags: [oc, ios]
categories: ios入门
---

## 概念

类是抽象，实例化是抽象的实现

## 代码

创建一个类-Person(人)

- 成员属性
    - name
        - 姓名
        - 字符串
        - NSString
    - age
        - 年龄
        - 整型
        - int
    - male
        - 性别
        - 布尔
        - BOOL

- 成员方法
    - hi
        - 表达您好

### Person.h

`Person.h` 负责创建成员变量，声明成员方法

``` c
//
//  Person.h
//  3面向对象-类实例化
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface Person : NSObject
@property(nonatomic, copy) NSString *name;
@property(nonatomic, assign) int age;
@property(nonatomic, assign) BOOL male;
- (void) hi;
@end

```

### Person.m

`Person.m`负责创建成员方法的具体实现

``` c
//
//  Person.m
//  3面向对象-类实例化
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person.h"

@implementation Person
- (void) hi {
    NSLog(@"hi");
}
@end
```

### Person使用

`.`来设置和获取成员属性
`[]`来调用成员方法

``` c
//
//  main.m
//  3面向对象-类实例化
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Person *person = [[Person alloc] init];
        person.name = @"hackingangle";
        person.age = 28;
        person.male = true;
        [person hi];
        NSLog(@"%@ %d %d", person.name, person.age, person.male);
    }
    return 0;
}
```

执行结果

``` shell
2017-07-26 13:05:49.282387+0800 3面向对象-类实例化[19296:1426503] hi
2017-07-26 13:05:49.282997+0800 3面向对象-类实例化[19296:1426503] hackingangle 28 1
Program ended with exit code: 0
```