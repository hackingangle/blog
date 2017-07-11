---
title: scrapy-jobbole
date: 2017-07-11 20:40:48
tags: scrapy
---

# 开始爬取之前

在开始爬取之前，我们只是知道如何定义开始爬取的页面地址，然后分析这个页面里面的核心元素，使用`xpath`或者`css`提取器

现在让我们都学一些，如何爬取到所有的jobbole中的文章呢，这里介绍的是通过列表页面爬取的方式

我们没有介绍:
1. 如何从列表开始页面分析到每一个资源的地址
1. 将这个地址告诉给scrapy的下载器，通过scrapy下载器来下载、分析
1. 提取下一页链接，传递到scrapy处理

## 处理url请求
``` python

# python3
from urllib import parse
# python2
# import urlparse

# 第一个参数是带有主域名的任意url，第二个参数是uri
parse.urljoin(response.url, nextUrl)
```

## 构建请求

1. 提取所有资源链接
``` shell
articleUrls = response.css("#archive .floated-thumb .post-thumb a::attr(href)").extract()
```

1. 提取到资源url，发送到scrapy下载分析
``` python
from scrapy.http import Request
yield Request(url=articleUrl, callback=self.parse_detail)
```

其中`callback`指定的`self.parse_detail`是设定的回调方法，用来对这个请求的返回结果的处理回调

1. 提取到下一页，发送到scrapy下载分析
``` python
nextUrl = response.css("a.next.page-numbers::attr(href)").extract_first("")
if nextUrl:
    yield Request(url=parse.urljoin(response.url, nextUrl), callback=self.parse)
```


# 知识点

## python
- 强转
    - int(XX)
