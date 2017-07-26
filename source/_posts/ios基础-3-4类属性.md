---
title: ios基础-3-4类属性
date: 2017-07-26 22:41:41
tags: ios
---

创建属性在.h文件中

@property来指定属性，内部限定涉及：多线程、内存分配释放

- 线程原子性
    - nonatomic
        - 不涉及多线程间数据交互，一般使用非原子性
    - atomic
        - 多线程保护机制，保证其原子性，速度慢
- assign 
    - 基础数据类型简单复制:int, float, double, BOOL...
- copy
    - NSString
        - 建立一个索引计数为1的对象，释放旧的对象
- retain
    - 除了NSString之外的
        - 与`copy`区别，`copy`复制一个字符串之后，地址发生了变化，
        同时内容是相同的，如果执行`retain`则旧的对象不发生变化，简单来说:
        `copy`内容进行操作，`retain`是指针的拷贝，之后内存管理`ARC`详细介绍
- strong weak
    - UI 控件经常使用
    - `强持有，交叉引用`!!!不懂哇