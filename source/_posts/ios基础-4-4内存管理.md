---
title: ios基础-4-4-内存管理
date: 2017-07-27 23:32:41
tags: ios
---

## 基础概念
- 内存
    - 计算机组成结构中的存储器，用来存储程序和数据
    - 由存储单元构成
- 指针
    - 类实例化后，申请了内存空间，指针就是一个指向内存空间的标示，存储了内存空间的地址
- 堆
    - 程序员申请的内存的存储单元
    - 先进先出
- 栈
    - 系统申请的内存的存储单元
    - 先进后出

## 内存管理机制

内存为什么需要管理？

内存空间无论在pc下还是在手机下面都是有限的，通常几个G。

我们需要拥有一定的机制，让不使用的内存及时释放，这样系统的速度不会受到影响。

OC中受到管控的内存使用场景：

1. NSObject类的所有子类
1. 其他语言c,c++申请的空间

不受内存管控的场景：

1. int,BOOL,char...等基础数据类型

## 内存警告

警告几次无用，IOS将直接自动收回内存，kill掉当前进程，导致app被kill

## ARC模式

编译器特征，释放程序员内存管理的精力：没有强指针指向对象，对象会被销毁

对象存储的基本结构：指针、索引、引用计数

- 引用计数
    - 对象被引用的次数，用4bytes=32bit来存
    - 初始值为0
    - alloc,new,copy 导致加1
    - retain 导致加1
    - release  导致减1
    - 如果为0，则释放
   
当引用技数为0时候，自动调用`dealloc`方法，被动调用

## 自动释放池
当autorealeasepool中出来的时候，对所有对象自动调用一次release，使得引用计数为0，自动回收内存空降

## Runtime

C语言中在编译时期，就知道了调用顺序

- OC语言在编译期并不知道调用顺序，是消息发送机制
    - 只要一个方法声明了，及时没有实现，在编译期也不会报错，在动态执行的时候报错
        - 比如：delegate，委托代理，委托代理中的方法有可能实现了，有可能没实现，每次调用前需要检查是否方法已经实现了
            - 委托代理中的方法有可能实现，也有可能没实现
### Runtime原理

OC如何通过发送消息实现动态调用的？

在调用的时候，会转成一个发送消息，如下：

``` c
[student learning];

student_msgSend(obj,@selector(doSomething));
```

说到消息，不得不看下OC下类的结构！

``` c
struct obj_class {
    Class isa;
    Class super_class;
    const char *name;
    long version;
    long info;
    long instance_size;
    struct obj_ivar_list *ivars;
    struct obj_method_list **methodLists;
    struct obj_cache *cache;
    struct objc_protocol_list *protocols;
}
```

- isa
    - 最重要的指针
    - 指向静态class，存储的是静态方法
- methodLists
    - 当前实现可以调用的方法列表
- cache
    - 最近使用的方法的指针
    - 快速、方便、高效的访问变量
    
步骤：

1. 正常的方法转消息发送
1. 可以通过obj对象指针可以找到我们的class
    - isa->Class
1. 先在缓存中找方法是否存在
    - cache
1. 没有找到，则在方法列表中找
    - methodLists
1. 找到后
    - 执行方法
    - 加入到cache缓存
    
## 使用场景

1. 程序运行中，动态添加(修改)方法和属性
1. 遍历类中所有方法和属性

## RunLoop

- 核心：使得程序（Thread）能够一直接收数据
- 处理各种事件的循环
- 不停处理调动工作和输入事件
- 优点
    - 节省cpu效率
    - 使得线程由工作忙，没工作空闲

## 指针

野指针：指向不可用内存，会导致crash
空指针：内存为空