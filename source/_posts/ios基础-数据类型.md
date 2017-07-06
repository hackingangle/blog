---
title: ios基础-3-数据类型
date: 2017-07-06 08:11:41
tags: ios
---

# 数据类型
``` c
//
//  main.m
//  New Application For OC
//
//  Created by wanggang on 2017/7/4.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        int a = 1;
        float b = 1.1;
        BOOL c = YES;
        BOOL d = NO;
        char e = 'a';
        double f = 1.1111;
        NSString *g = @"Hi";
        id h = @"Hi";
        NSLog(@"%d %f %d %d %c %g %@ %@", a, b, c, d, e, f, g, h);
    }
    return 0;
}

```

先上程序，再说知识点。

基础数据类型：

1. int 整型
1. float 浮点型
1. BOOL 布尔
1. char 字符
1. double 双精度浮点型
1. NSString 字符串对象
1. id 任意对象


注意！

我们这里介绍了2个大类：`基础`和`对象`类型

对象类型都必须用 `*objectName`来创建，其中id表示任意类型不用加`*`

另外格式化字符在NSLog中，其中`@`表示任意对象，`g`表示科学技术

# 限定词&强转

``` c
//
//  main.m
//  2-DataType
//
//  Created by wanggang on 2017/7/6.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        int a = 123456789012345;
        long long int b = 123456789012345;
        NSLog(@"%d %lld", a, b);
        // -2045911175 123456789012345
        unsigned int c = 1;
        unsigned int d = -1;
        NSLog(@"%d %d", c, d);
        // 1 -1
        NSLog(@"%u %u", c, d);
        // 1 4294967295
        float e = 1.1;
        int f = (int)e;
        NSLog(@"%f %d", e, f);
        // 1.100000 1
        int g = 1;
        float h = (float)g;
        NSLog(@"%d %f", g, h);
        // 1 1.000000
    }
    return 0;
}
```
## 限定词使用场景

1. 大数字
    - a,b所示，当大数字步不使用限定词long long的时候，会被计算成负数，不满足期望！
1. 特定数字类型
    - 年龄数据不可以为负数，声明无符号整型，当不为无符号时候被转为大数字
    - 如果用%d输出，依旧可以输出原来的数字(负数)

限定词列表:

- long
- long long
- short
- signed
- unsigned

## 强转

使用`(int)`或`(float)`等可以进行类型强转

- int->float
    - 增加精度
- float->int
    - 丢失精度


## 变量的作用域
``` c
//
//  main.m
//  变量作用域
//
//  Created by wanggang on 2017/7/6.
//  Copyright © 2017年 hackingangle. All rights reserved.
//

#import <Foundation/Foundation.h>

int a = 3;
int main(int argc, const char * argv[]) {
    NSLog(@"方法体内部1:%d", a);
    int a = 2;
    @autoreleasepool {
        int a = 1;
        NSLog(@"语句块内部:%d", a);
    }
    NSLog(@"方法体内部2:%d", a);
    return 0;
}

```

``` shell
方法体内部1:3
语句块内部:1
方法体内部2:2
```

总结：

1. 三个范围
    - 方法体外
    - 方法内
    - 语句块内
1. 内部变量生效优先级大于外部变量
1. 内部变量不会改变外部变量的值

