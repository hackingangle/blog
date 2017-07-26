---
title: ios基础-3-6继承多态
date: 2017-07-26 23:30:41
tags: ios
---

## 继承

继承父类所有的属性和方法

## 多态

相同的方法，不同的表现行为在不同的场景下

``` c 
//
//  main.m
//  3-6继承多态
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"
#import "Student.h"
#import "Teacher.h"
int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Person *person = [[Person alloc] init];
        [person hi];
        // 继承
        Student *stu = [[Student alloc] init];
        [stu hi];
        Teacher *teacher = [[Teacher alloc] init];
        [teacher hi];
        // 多态
//        Person *person2 = [[Student alloc] init];
        Person *person2 = [[Teacher alloc] init];
        [person2 my];
    }
    return 0;
}

//
//  Person.h
//  3-6继承多态
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface Person : NSObject
-(void)hi;
-(void)my;
@end

//
//  Person.m
//  3-6继承多态
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person.h"

@implementation Person
-(void)hi{
    NSLog(@"hi in person");
}
-(void)my{
    NSLog(@"my in person");
}
@end


//
//  Student.h
//  3-6继承多态
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"

@interface Student : Person

@end

//
//  Student.m
//  3-6继承多态
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Student.h"

@implementation Student

-(void)my{
    NSLog(@"my in student");
}

@end


//
//  Teacher.h
//  3-6继承多态
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person.h"

@interface Teacher : Person

@end


//
//  Teacher.m
//  3-6继承多态
//
//  Created by wanggang on 2017/7/26.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Teacher.h"

@implementation Teacher

-(void)my{
    NSLog(@"my in teacher");
}

@end

```