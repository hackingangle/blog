---
title: ios基础-4-1-NSString
date: 2017-07-27 13:07:41
tags: [oc, ios]
categories: ios入门
---

字符串创建、比较、搜索、获取子字符串、转换大小写、转换首字母、增删改查、trim

``` c
//
//  main.m
//  4-1字符串
//
//  Created by wanggang on 2017/7/27.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        // 1.创建字符串
        NSString *str1 = [[NSString alloc] init];
        NSString *str2 = @"str2";
        NSLog(@"%@ %@", str1, str2);
        // 2.基础数据类型->字符串
        NSString *str3 = [[NSString alloc] initWithFormat:@"%f", 2.2];
        NSString *str4 = [[NSString alloc] initWithFormat:@"%d", 2];
        NSLog(@"%@ %@", str3, str4);
        // 字符串->基础数据类型
        NSLog(@"%f %d", str3.floatValue, str4.intValue);
        // 3.字符串的比较
        NSString *str5 = @"abc";
        NSString *str6 = @"abc";
        NSString *str7 = [[NSString alloc] initWithFormat:@"%@", @"abc"];
        // 相同的值
        if ([str5 isEqualToString:str6]) {
            NSLog(@"str5 str6 is equal.");
        }
        // 相同的指针，相同的值
        if (str6 == str5) {
            NSLog(@"str5 str6 all equal(value, quote pointer)");
        }
        // 不同的指针，相同的值
        if (str5 == str7) {
            NSLog(@"str5 str7 all equal(value, quote pointer)");
        }
        // 4.字符串大小写，首字母大写
        NSString *str8 = @"hacking angle";
        NSLog(@"%@", [str8 uppercaseString]);
        NSLog(@"%@", [str8 lowercaseString]);
        NSLog(@"%@", [str8 capitalizedString]);
        // 5.字符串内容索引
        NSString *str9 = @"abcedf12345";
        NSString *str10 = @"edf1";
        NSRange range;
        range = [str9 rangeOfString:str10];
        NSLog(@"lenth = %lu  location = %lu", range.length, range.location);
        // 6.字符串位置索引
        NSString *str11 = @"abcdef12345";
        NSRange range2;
        range2.location = 1;
        range2.length = 2;
        NSString *str12 = [str11 substringWithRange:range2];
        NSLog(@"%@", str12);
        // 7.字符串增删改查
        NSString *str13 = @"a";
        NSString *str14 = [str13 stringByAppendingString:@"b"];
        NSString *str15 = [str13 stringByAppendingFormat:@"%d %f", 1, 1.1];
        NSLog(@"%@ %@ %@", str13, str14, str15);
        NSString *str16 = [str13 stringByReplacingOccurrencesOfString:@"a" withString:@"e"];
        NSLog(@"%@", str16);
        // 8.可变字符串
        NSMutableString *str17 = [[NSMutableString alloc] initWithString:@"abc"];
        [str17 appendString:@"1"];
        [str17 insertString:@"@hackingangle..." atIndex:1];
        [str17 appendFormat:@"%d", 10000];
        NSLog(@"%@", str17);
        // 9.字符串内容检查
        NSString *str18 = @"http://www.baidu.com/logo.png";
        if ([str18 containsString:@"http://"]) {
            NSLog(@"http协议");
        }
        if ([str18 hasPrefix:@"http://"]) {
            NSLog(@"以http开头");
        }
        if ([str18 hasSuffix:@".png"]) {
            NSLog(@"是一张图");
        }
        // 10去空白
        NSString *str19 = @"  aa  ";
        NSString *str20 = [str19 stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceCharacterSet]];
        NSLog(@"%@ %@", str19, str20);
        NSLog(@"%lu %lu", str19.length, str20.length);
    }
    return 0;
}

```