---
title: ios基础-4-3-NSDictionary
date: 2017-07-27 23:07:41
tags: ios
---

``` c
//
//  main.m
//  4-3NSDictionary
//
//  Created by wanggang on 2017/7/27.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // 初始化
        NSDictionary *dict1 = [[NSDictionary alloc] init];
        NSDictionary *dict2 = [[NSDictionary alloc] initWithObjectsAndKeys:@"2.2", @"apple", @"1.1", @"bnana", nil];
        NSDictionary *dict3 = @{@"beijing":@"111.2",@"tianjian":@"38.2"};
        NSLog(@"%@ %@ %@", dict1, dict2, dict3);
        // 增删改查
        NSString *str1 = [dict2 valueForKey:@"apple"];
        NSArray *arr1 = [dict3 allKeys];
        NSArray *arr2 = [dict3 allValues];
        NSLog(@"%@", str1);
        NSLog(@"%@ %@", arr1, arr2);
        for (int i=0;i<arr1.count;i++) {
            NSLog(@"%@", [dict3 valueForKey:[arr1 objectAtIndex:i]]);
        }
        // 遍历是key
        for (NSString *item in dict3) {
            NSLog(@"%@ %@", item, [dict3 valueForKey:item]);
        }
        // 字典数组混合使用
        NSArray *arr3 = @[@"a", @"b"];
        NSDictionary *dict4 = [[NSDictionary alloc] initWithObjectsAndKeys:@"v1", @"kk1", @"v22", @"kkk2", arr3, @"kkkk3", nil];
        NSLog(@"%@ %@", arr3, dict4);
        for (int i=0;i<[dict4 allKeys].count;i++){
            NSLog(@"%@", [dict4 valueForKey:[[dict4 allKeys] objectAtIndex:i]]);
        }
        // 字典的增删改查
        NSMutableDictionary *dict5 = [[NSMutableDictionary alloc] init];
        [dict5 setValue:@"hackingangle" forKey:@"username"];
        [dict5 setValue:@"27" forKey:@"age"];
        NSLog(@"%@", dict5);
        [dict5 setValue:@"ha" forKey:@"username"];
        NSLog(@"%@", dict5);
        [dict5 removeObjectForKey:@"age"];
        NSLog(@"%@", dict5);
        [dict5 removeAllObjects];
        NSLog(@"%@", dict5);
    }
    return 0;
}

```
