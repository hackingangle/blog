---
title: ios基础-5-5KVC&KVO
date: 2017-08-02 21:51:41
tags: ios
---

## what

### kvc

> kvc = Key Value Coding

- 动态设置
    - setValue forKey
    - setValue forKeyPath

- 动态读取
    - valueForKey
    - valueForKeyPath

## kvo

> kvo = 监听键值属性变化

- 步骤
    - 注册
    - 观察
    - 移除观察

## how

### kvc

``` c
//
//  main.m
//  5-5KVC
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Person *person1 = [[Person alloc] init];
        Person *person2 = [[Person alloc] init];
        [person1 setValue:@"hackingangle" forKey:@"username"];
        [person1 setValue:@"29" forKey:@"age"];
        [person2 setValue:@"wanggang" forKeyPath:@"username"];
        [person2 setValue:@"29" forKey:@"age"];
        NSLog(@"%@ %@", [person1 valueForKeyPath:@"username"], [person1 valueForKey:@"age"]);
    }
    return 0;
}

//
//  Person.h
//  5-5KVC
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface Person : NSObject
@property(nonatomic, assign) int age;
@property(nonatomic, copy) NSString* username;
@end

//
//  Person.m
//  5-5KVC
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person.h"

@implementation Person

@end

```

output

``` shell
2017-08-02 22:05:29.528244+0800 5-5KVC[23476:2071822] hackingangle 29
Program ended with exit code: 0
```

### kvo

一般套路：
``` c
//
//  main.m
//  5-6KVO
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "KVO.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        KVO *kvo = [[KVO alloc] init];
        [kvo testKVO];
    }
    return 0;
}

//
//  Person.h
//  5-6KVO
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface Person : NSObject
@property(nonatomic, assign) int age;
@end

//
//  Person.m
//  5-6KVO
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Person.h"

@implementation Person

@end

//
//  KVO.h
//  5-6KVO
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Person.h"

@interface KVO : NSObject
@property(nonatomic, strong) Person *person;
-(void) testKVO;
@end

//
//  KVO.m
//  5-6KVO
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "KVO.h"

@implementation KVO
-(void) testKVO{
    self.person = [[Person alloc] init];
    [self.person setAge:29];
    [self.person addObserver:self forKeyPath:@"age" options:NSKeyValueObservingOptionNew | NSKeyValueObservingOptionOld context:nil];
    [self.person setAge:28];
}
-(void)dealloc{
    [self.person removeObserver:self forKeyPath:@"age"];
}
-(void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context {
    NSLog(@"年龄变化");
}
@end

```

## 定时器+kvo

``` c
//
//  main.m
//  5-6KVOStock
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "KVO.h"

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        KVO *k = [[KVO alloc] init];
        [k testKVO];
    }
    return 0;
}

//
//  Stock.h
//  5-6KVOStock
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

@interface Stock : NSObject
@property(nonatomic, assign) float price;
@end

//
//  Stock.m
//  5-6KVOStock
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "Stock.h"

@implementation Stock

@end

//
//  KVO.h
//  5-6KVOStock
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>
#import "Stock.h"

@interface KVO : NSObject
@property(nonatomic, strong) Stock* stock;
@property(nonatomic, strong) NSTimer* timer;
-(void)testKVO;
@end

//
//  KVO.m
//  5-6KVOStock
//
//  Created by wanggang on 2017/8/2.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import "KVO.h"

float currentPrice;

@implementation KVO
-(void)testKVO {
    currentPrice = 3.1;
    self.stock = [[Stock alloc] init];
    [self.stock setPrice:currentPrice];
    // 注册
    [self.stock addObserver:self forKeyPath:@"price" options:NSKeyValueObservingOptionNew|NSKeyValueObservingOptionOld context:nil];
    self.timer = [NSTimer timerWithTimeInterval:1 target:self selector:@selector(changePrice) userInfo:nil repeats:true];
    [[NSRunLoop currentRunLoop] addTimer:self.timer forMode:NSDefaultRunLoopMode];
    [[NSRunLoop currentRunLoop] run];
}
// 注销
-(void)dealloc{
    [self.stock removeObserver:self forKeyPath:@"price"];
}
-(void)changePrice{
    currentPrice += 0.1;
    [self.stock setPrice:currentPrice];
    NSLog(@"up price , %f", currentPrice);
}
// 观察
-(void)observeValueForKeyPath:(NSString *)keyPath ofObject:(id)object change:(NSDictionary<NSKeyValueChangeKey,id> *)change context:(void *)context {
    NSString *oldPrice = [change valueForKey:@"old"];
    NSString *newPrice = [change valueForKey:@"new"];
    if (newPrice.floatValue > oldPrice.floatValue) {
        NSLog(@"股票价格上涨，买入!!");
    }
}
@end
```

output

``` shell
2017-08-02 22:38:06.749620+0800 5-6KVOStock[23586:2102683] 股票价格上涨，买入!!
2017-08-02 22:38:06.749928+0800 5-6KVOStock[23586:2102683] up price , 3.200000
2017-08-02 22:38:07.745757+0800 5-6KVOStock[23586:2102683] 股票价格上涨，买入!!
2017-08-02 22:38:07.745800+0800 5-6KVOStock[23586:2102683] up price , 3.300000
2017-08-02 22:38:08.747685+0800 5-6KVOStock[23586:2102683] 股票价格上涨，买入!!
2017-08-02 22:38:08.747755+0800 5-6KVOStock[23586:2102683] up price , 3.400000
2017-08-02 22:38:09.746796+0800 5-6KVOStock[23586:2102683] 股票价格上涨，买入!!
...
```