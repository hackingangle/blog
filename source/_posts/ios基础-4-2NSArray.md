---
title: ios基础-4-2-NSArray
date: 2017-07-27 15:22:41
tags: ios
---

- 有顺序
- 有下标

## 创建

``` c
//
//  main.m
//  4-2NSArray
//
//  Created by wanggang on 2017/7/27.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        NSArray *arr1 = [[NSArray alloc] init];
        NSArray *arr2 = [[NSArray alloc] initWithObjects:@"a", @"b", @"c", nil];
        NSArray *arr3 = @[@"a", @"b", @"c"];
        NSLog(@"%@ %@ %@", arr1, arr2, arr3);
    }
    return 0;
}

```

## 操作

- 增删改查
- 长度
- 包含

``` c
//
//  main.m
//  4-2NSArray
//
//  Created by wanggang on 2017/7/27.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        NSArray *arr1 = [[NSArray alloc] init];
        NSArray *arr2 = [[NSArray alloc] initWithObjects:@"a", @"b", @"c", nil];
        NSArray *arr3 = @[@"a", @"b", @"c"];
        NSLog(@"%@ %@ %@", arr1, arr2, arr3);
        // 查询
        NSString *str1 = [arr3 firstObject];
        NSString *str2 = [arr3 lastObject];
        NSString *str3 = [arr3 objectAtIndex:1];
        NSLog(@"%@ %@ %@", str1, str2, str3);
        NSUInteger position = [arr3 indexOfObject:@"b"];
        NSLog(@"%lu", position);
        // 长度
        NSLog(@"length = %lu", arr2.count);
        // 包含
        NSLog(@"%d %d", [arr2 containsObject:@"a"], [arr2 containsObject:@"aa"]);
    }
    return 0;
}

```

不知道大家注意没有？上述没有修改、删除以及新增的操作，为什么呢？

因为我们只有在可变数组下面才能操作。

``` c
//
//  main.m
//  4-2NSArray
//
//  Created by wanggang on 2017/7/27.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        NSArray *arr1 = [[NSArray alloc] init];
        NSArray *arr2 = [[NSArray alloc] initWithObjects:@"a", @"b", @"c", nil];
        NSArray *arr3 = @[@"a", @"b", @"c"];
        NSLog(@"%@ %@ %@", arr1, arr2, arr3);
        // 查询
        NSString *str1 = [arr3 firstObject];
        NSString *str2 = [arr3 lastObject];
        NSString *str3 = [arr3 objectAtIndex:1];
        NSLog(@"%@ %@ %@", str1, str2, str3);
        NSUInteger position = [arr3 indexOfObject:@"b"];
        NSLog(@"%lu", position);
        // 长度
        NSLog(@"length = %lu", arr2.count);
        // 包含
        NSLog(@"%d %d", [arr2 containsObject:@"a"], [arr2 containsObject:@"aa"]);
        
        // 删除和修改必须依赖mutableArray
        NSMutableArray *arr4 = [[NSMutableArray alloc] initWithObjects:@"aa", @"bb", @"cc", @"dd", nil];
        // 新增
        [arr4 addObject:@"ee"];
        NSLog(@"%@", arr4);
        [arr4 insertObject:@"MMMMM" atIndex:1];
        NSLog(@"%@", arr4);
        [arr4 removeLastObject];
        NSLog(@"%@", arr4);
        [arr4 removeObjectAtIndex:1];
        NSLog(@"%@", arr4);
        [arr4 removeObject:@"cc"];
        NSLog(@"%@", arr4);
        [arr4 removeAllObjects];
        NSLog(@"%@", arr4);
        
        // NSString->NSArray
        NSString *str4 = @"1,2,3,4,5,6,7";
        NSArray *arr5 = [str4 componentsSeparatedByString:@","];
        NSLog(@"%@", arr5);
        NSMutableString *str5 = [[NSMutableString alloc] init];
        for (NSString *item in arr5) {
            [str5 appendFormat:@"%@@", item];
        }
        NSLog(@"%@", str5);
        
        // NSArray<->NSMutableArray
        NSArray *arr6 = @[@"a",@"b",@"c"];
//        NSMutableArray *arr7 = arr6;
        NSMutableArray *arr7 = [[NSMutableArray alloc] initWithArray:arr6];
        NSArray *arr8 = arr7;
        NSLog(@"%@ %@ %@", arr6, arr7, arr8);
    }
    return 0;
}

```