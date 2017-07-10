---
title: scrapy-使用
date: 2017-07-10 21:50:21
tags: scrapy
---

## 初始化virtualenv环境

``` shell
## init 环境容器，从而保障独立的依赖
mkvirtualenv --python=/usr/bin/python3 scrapy_python3
## 切换到环境
workon scrapy_python3
## 安装scrapy
pip install scrapy
```

## 创建工程

``` shell
## 创建工程
scrapy startproject ArticleSpider
## 进入工程目录
cd ArticleSpider
## 创建爬虫
scrapy genspider jobbole blog.jobbole.com
```

## 分析目录结构

![scrapy项目结构](/images/scrapy/scrapy项目结构.png)

## pycharm 设定解析器

![python解析器](/images/scrapy/python解析器.jpeg)

## spider代码分析

``` python
# -*- coding: utf-8 -*-
import scrapy


class JobboleSpider(scrapy.Spider):
    # 爬虫名称
    name = 'jobbole'
    # url爬取域名白名单
    allowed_domains = ['blog.jobbole.com']
    # 开启爬取url列表
    start_urls = ['http://blog.jobbole.com/']

    def parse(self, response):
        pass
```

`JobboleSpider`继承了`scrapy.Spider`类，在`scrapy.Spider`类中定义如下：

![scrapy.spider](/images/scrapy/scrapy.spider.jpeg)

可以知道，scrapy把每一个`start_urls`中定义的url，创建成`Request`对象，通过`yield`关键词下发给`scrapy下载器`，
从而实现高并发、高效率下载。

## pycharm调试方法

pycharm执行、调试程序必须有一个入口方法，我们需要定义一个程序，发起`scrapy crawl xxx`命令来启动爬虫，具体做法如下：

- 新增一个main.py在爬虫工程目录/ArticleSpider:

``` python
from scrapy.cmdline import execute
execute(['scrapy', 'crawl', 'jobbole'])
```

- 打一个断点

断点是为了更好的调试程序，反馈在这个语句中上下文的变量情况

![调试断点](/images/scrapy/调试断点.jpeg)

## 关闭robot协议

在`settings.py`中`ROBOTSTXT_OBEY`默认定义为`True`表示遵循robots协议，这样在协议中不允许访问的url我们就不能抓取了，
我们必须来关闭他。

> 很简单：ROBOTSTXT_OBEY = False


## 运行结果

![断点调试结果](/images/scrapy/断点调试结果.jpeg)

通过观察结果，我们可以知道，parse方法在下载器下载完毕页面后被调用，这里可以捕获到`Response`返回对象，
包含`Response`的全部信息，如：返回结果、返回状态码等等...

## 总结

我们花费了大量篇幅来介绍爬虫的基础知识、环境的准备等，今天终于开始爬虫的实践了，很激动...
在实战篇，将深入探讨爬虫的使用方法，原理以及高级设置等~~~
