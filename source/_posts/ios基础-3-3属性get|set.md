---
title: ios基础-3-3属性get|set
date: 2017-07-26 22:27:41
tags: ios
---

封装，内部变量的细节封装起来，暴露外部接口使用

``` c
//
//  main.m
//  3-3属性set|get
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Person *person = [[Person alloc] init];
        person.age = -29;
        NSLog(@"%d", person.age);
    }
    return 0;
}


//
//  Person.h
//  3-3属性set|get
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface Person : NSObject {
    NSString *name;
    int age;
}
-(void)setAge:(int)newAge;
-(int)age;
@end


//
//  Person.m
//  3-3属性set|get
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person.h"

@implementation Person
-(void)setAge:(int)newAge{
    if (newAge < 0) {
        newAge = 1;
    }
    age = newAge;
}
-(int)age {
    if (age > 0) {
        NSLog(@"茁壮成长");
    }
    return age;
}
@end
```
