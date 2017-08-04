---
title: scrapy-unicode_utf8_编码讲解
date: 2017-07-09 12:23:40
tags: scrapy
---

# 基础

## 起始-ASCII
计算机无论存储、传输、计算都是基于数字的，因为计算机只能识别0,1。

处理的最小单位是bit

美国人发明了计算机，在美国1个字节（最多表示255数字)就够了，所有ASCII标准是美国人的标准。

> 255数字由来：1byte=8bite=2^8=255

## 国之标准-GB2312
中国地大物博，文字丰富，常用字就远远超过了`255个字符`，所以中国人要有自己的标准-gb2312

在`gb2312`中，我们用2个字节代表1个汉字字符，这样最多可以有2^16=65535个表示范围，其中包含了ASCII包含的255个定义，
体现了向下兼容的软件设计思想

地球肯定不是只有中国和美国两个国家，这样美国国家根据自己的语言特色，都开发了自己的计算机语言标准

问题来了？

假设我是联合国秘书，如果我要在一篇文章里面包含10国语言，怎么在计算机输入，你要知道每个国家用1,2,3,4字节表示自己的语言都不一样，而且还有冲突占用情况

这样我们需要一个联合国的计算机标准，把每一个国家的标准都囊括进来

unicode运势而生

## 国际之标准-Unicode

Unicode分为2字节16bit版本和4字节32bit版本

这样，联合国秘书在一篇文章里面包含10国语言的问题解决了

但是又带来一个存储问题，因为英文1byte就可以表示了，但是实际存储或传输的时候还需要占用2byte或4byte，太不划算了，
要知道互联网下传输就是钱

我们来看一下区别在哪里？

1. A 用ASCII十进制位65，二进制为:0100 0001
1. A 用Unicode 16bit表示为:0000 0000 0100 0001

如果我是一家互联网公司的老板，基础的传输就占用了我双倍的预算，我肯定要推动技术的变革了！

动态修正浪费的字符情况-Utf8

## 节省开销-Utf8

- 可变字长的编码
    - 英文占用1byte
    - 汉字
        - 常用占用3byte
        - 生僻字占用4-6byte

实际的应用，随便打开一个网页源码，看看HTML指定的编码，一定是UTF8

![网页使用utf8编码](/images/scrapy/网页使用utf8编码.jpeg)

因为UTF8让我们实现了国际通用的显示标准的前提下，HTML中包含了大量的英文字符，这样我们使用可变字长编码节省了至少1倍的费用，没有人会拒绝相同效果下少花钱！

# Python中Unicode和Utf8

Unicode是最通用的，没有考虑到可变字长，非常适合在内存中使用

Utf8考虑了可变字长，非常时候`python`中源代码在磁盘上存储

所以Python中的编码转换的模型如下：

![python中编码转换模型](/images/scrapy/python中编码转换模型.jpeg)

上述模型中，我们可以知道Python中默认将字符串编码认为是unicode，这样如果代码中不是unicode的时候会带来编码异常

Python2 != Python3

在Python2中，我们需要指定源码的编码集合为Utf8，否则不支持中文：

``` python
# -*- coding:utf-8 -*-
str = "我喜欢python"
print str
```
不指定编码会报错
``` shell
SyntaxError: Non-ASCII character '\xe6' in file /Users/wanggang/python/p2/main.py on line 1, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details
```

通过上面报错可以知道，默认的编码集合为ASCII，当出现中文的时候，超过了ASCII的1个字节的256个代表能力的最大限制，所以有`\xe6`不能识别

我们来通过sys库看下默认的源码编码集合

## Python2
``` python
# -*- coding:utf-8 -*-
import sys
str = "我喜欢python"

print str
print sys.getdefaultencoding()
```
 
输出
``` shell
我喜欢python
ascii
```

可以知道，默认的编码集合为ascii，我们一定要指定编码集合

## Python3
``` python
import sys
str = "我喜欢python"

print(str)
print(sys.getdefaultencoding())
```

相同代码，在python3中可以不指定utf8编码集合，因为默认就是utf8，我们可以看一下输出：

``` shell
我喜欢python
utf-8
```

## 编码的转换

可以看到一些教材在讲解这个部分的时候弄的很复杂，还区分`wins`,`linux`,`mac`平台老讲解，笔者感觉搞得太复杂了

我们只需要记住以下知识点，就可以轻松进行编码的转换：

1. 编码转换两个方法
    - encode
        - 要求操作字符串的编码集合一定是`Unicode`的，为什么这样呢？
            - 非常简单，大家可以看看编码在python中的模型，上面已经介绍过，内存在的编码就是unicode的，所以我们要保证这一点
    - decode
        - 这个方法就是保证encode之前代码转成内存在要求的`Unicode`
        - 各个平台在这个部分有较大的差别
1. 在源码中是否可以直接指定编码为`Unicode`呢
    - u"我喜欢python"
1. 最重要一点
    - python3中，下述表示无区别
        - "我喜欢python"
        - u"我喜欢python"

> python3中把所有的字符串都自动转换成Unicode，不用如python2中一样使用decode来转换了

## python2
``` python
# -*- coding:utf-8 -*-
str = "我喜欢python"
print str.decode("utf-8").encode("gb2312")
# 直接声明unicode
str = u"我喜欢python"
print str.encode("gb2312")
```
``` shell
��ϲ��python
��ϲ��python
```

## python3
``` python
str = "我喜欢python"
print(str.encode("gb2312"))
# 直接声明unicode
str = u"我喜欢python"
print(str.encode("gb2312"))
```
``` shell
b'\xce\xd2\xcf\xb2\xbb\xb6python'
b'\xce\xd2\xcf\xb2\xbb\xb6python'
```
