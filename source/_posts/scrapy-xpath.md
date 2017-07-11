---
title: scrapy-xpath
date: 2017-07-11 07:30:22
tags: scrapy
---

## what

- xpath使用路径表达式在xml和html进行导航
- xpath包含标准函数库
- xpath是w3c标准

### 节点关系

- 父节点
    - html是head父节点
- 子节点
    - head是html子节点
- 同袍节点
    - head,body
- 先辈节点
    - meta先辈节点：head,html
- 后代节点
    - meat是head,html的后代节点

### 语法

- article
    - 所有article 所有子节点
- /article
    - 根元素 article
- article/a
    - 所有article 子元素a元素
- //div
    - 所有div 所有子节点(无论在文档中任何位置)
- article//div
    - 所有article 后代div无论位置
- //@class
    - 所有class的属性
- /article/div[1]
    - article子元素第一个div
- /article/div[last()]
    - article子元素最后一个div
- /article/div[last()-1]
    - article子元素倒数第二个div
- //div[@lang]
    - 所有有lang属性的div
- //div[@lang='eng']
    - 所有lang属性等于eng的div
- //div/*
    - 属于div元素所有子节点
- //*
    - 所有元素
- //div[@*]
    - 所有带属性的div
- //div/a|//div/p
    - 所有div中的a和p元素
- //span|//ul
    - 文档中的span和ul元素
- article/div/p|//span
    - 所有属于article元素的div元素的p元素以及文档中的所有span元素

## how

### In Scrapy

使用response对象中自带的xpath解析器:`response.xpath()`

指定xpath后调用值，只需要增加`text()`，如：`response.xpath('/div/text()')`

浏览器直接复制出来的xpath为什么定位不到元素？

因为浏览器复制出来的元素是包含了js运行结果的，也就是说，如果js增加了一些node节点，那我们在复制的时候也复制了这部分的node节点。然而，
我们可以知道，通过response返回的结果中是不包含js运行结果的，所以说直接浏览器复制的xpath路径是有可能提取不到元素的。

``` python
# -*- coding: utf-8 -*-
import scrapy
import re


class JobboleSpider(scrapy.Spider):
    # 爬虫名称
    name = 'jobbole'
    # url爬取域名白名单
    allowed_domains = ['blog.jobbole.com']
    # 开启爬取url列表
    start_urls = ['http://blog.jobbole.com/111742/']

    def parse(self, response):
        # 提取
        title = response.xpath('//div[@class="entry-header"]/h1/text()')
        # 解压缩转数组
        titleArr = title.extract()
        # 提取第一个为标题
        ret_title = titleArr[0]
        # 提取创建日志
        ret_create_date = response.xpath("//p[@class='entry-meta-hide-on-mobile']/text()").extract()[0].strip().replace(
            " ·", "")
        # 提取点赞
        ret_praise_nums = int(response.xpath("//span[contains(@class, 'vote-post-up')]/h10/text()").extract()[0])
        # 收藏
        strFavNums = response.xpath("//span[contains(@class, 'bookmark-btn')]/text()").extract()[0]
        reHandle = re.match(".*(\d+).*", strFavNums)
        if reHandle:
            ret_fav_nums = reHandle.group(1)
        # 评论
        strCommentNums = response.xpath("//a[@href='#article-comment']/span/text()").extract()[0]
        reHandle = re.match(".*(\d+).*", strCommentNums)
        if reHandle:
            ret_comment_nums = reHandle.group(1)
        # 正文
        ret_content = response.xpath("//div[@class='entry']/text()").extract()[0]

        # 标签
        tagList = response.xpath("//p[@class='entry-meta-hide-on-mobile']/a/text()").extract()
        tagList = [element for element in tagList if not element.strip().endswith("评论")]
        ret_tags = ",".join(tagList)
        pass
```

看一下运行结果，分析出来的属性以`ret_`开头，如下：

![xpath结果](/images/scrapy/xpath结果.jpeg)

### 上述在Scrapy的spider程序中调试每次都需要等待？

这里介绍一个不需要等待的调试方法`scrapy shell`

好吧，我们来调试一下，命令行输入：`scrapy shell http://blog.jobbole.com/111742/`

在交互式命令行中调试代码如下：

``` python
import re
# 提取
title = response.xpath('//div[@class="entry-header"]/h1/text()')
# 解压缩转数组
titleArr = title.extract()
# 提取第一个为标题
strTitle = titleArr[0]
# 提取创建日志
strCreateDate = response.xpath("//p[@class='entry-meta-hide-on-mobile']/text()").extract()[0].strip().replace(" ·", "")
# 提取点赞
intPraiseNums = int(response.xpath("//span[contains(@class, 'vote-post-up')]/h10/text()").extract()[0])
# 收藏
strFavNums = response.xpath("//span[contains(@class, 'bookmark-btn')]/text()").extract()[0]
reHandle = re.match(".*(\d+).*", strFavNums)
if reHandle:
    print(reHandle.group(1))
# 评论
strCommentNums = response.xpath("//a[@href='#article-comment']/span/text()").extract()[0]
reHandle = re.match(".*(\d+).*", strCommentNums)
if reHandle:
    print(reHandle.group(1))
# 正文
content = response.xpath("//div[@class='entry']/text()").extract()[0]

# 标签
tagList = response.xpath("//p[@class='entry-meta-hide-on-mobile']/a/text()").extract()
tagList = [element for element in tagList if not element.strip().endswith("评论")]
tags = ",".join(tagList)
```

知识点：
- extract
    - 提取selecter中的数据
- strip
    - 去掉空白
- replace
    - 替换字符
- contains
    - 包含属性
- 数组过滤
    - `[element for element in tagList if not element.strip().endswith("评论")]`
- 数组转字符串
    - ",".join(tagList)
