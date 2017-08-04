---
title: scrapy-itemloader
date: 2017-07-12 09:31:44
tags: scrapy
---

# what
上述的item中每个属性的提取规则定义在爬虫中了，不方便进行代码的复用和管理。

# how
``` python
# -*- coding: utf-8 -*-
import scrapy
import re
from scrapy.http import Request
from urllib import parse
from ArticleSpider.items import JobBoleArticleItem
from ArticleSpider.utils.common import get_md5
from scrapy.loader import ItemLoader
from ArticleSpider.items import ArticleItemLoader


class JobboleSpider(scrapy.Spider):
    # 爬虫名称
    name = 'jobbole'
    # url爬取域名白名单
    allowed_domains = ['blog.jobbole.com']
    # 开启爬取url列表
    start_urls = ['http://blog.jobbole.com/all-posts/']

    def parse(self, response):
        articleUrlHandles = response.css("#archive .floated-thumb .post-thumb")
        for articleUrlHandle in articleUrlHandles:
            articleUrl = articleUrlHandle.css("a::attr(href)").extract_first("")
            articleBanner = articleUrlHandle.css("a img::attr(src)").extract_first("")
            yield Request(url=articleUrl, callback=self.parse_detail, meta={"front_image_url":parse.urljoin(response.url, articleBanner)})
        nextUrl = response.css("a.next.page-numbers::attr(href)").extract_first("")
        if nextUrl:
            yield Request(url=parse.urljoin(response.url, nextUrl), callback=self.parse)
        return

    def parse_detail(self, response):
        # 实例化JobBoleArticleItem
        # article_item = JobBoleArticleItem()
        # meta获取banner
        front_image_url = response.meta.get("front_image_url", "")
        # # 提取
        # title = response.xpath('//div[@class="entry-header"]/h1/text()')
        # # 解压缩转数组
        # titleArr = title.extract()
        # # 提取第一个为标题
        # ret_title = titleArr[0]
        # # 提取创建日志
        # ret_create_date = response.xpath("//p[@class='entry-meta-hide-on-mobile']/text()").extract()[0].strip().replace(
        #     " ·", "")
        # # 提取点赞
        # ret_praise_nums = int(response.xpath("//span[contains(@class, 'vote-post-up')]/h10/text()").extract()[0])
        # # 收藏
        # strFavNums = response.xpath("//span[contains(@class, 'bookmark-btn')]/text()").extract()[0]
        # reHandle = re.match(".*(\d+).*", strFavNums)
        # if reHandle:
        #     ret_fav_nums = int(reHandle.group(1))
        # else:
        #     ret_fav_nums = 0
        # # 评论
        # strCommentNums = response.xpath("//a[@href='#article-comment']/span/text()").extract()[0]
        # reHandle = re.match(".*(\d+).*", strCommentNums)
        # if reHandle:
        #     ret_comment_nums = int(reHandle.group(1))
        # else:
        #     ret_comment_nums = 0
        # # 正文
        # ret_content = response.xpath("//div[@class='entry']/text()").extract()[0]
        #
        # # 标签
        # tagList = response.xpath("//p[@class='entry-meta-hide-on-mobile']/a/text()").extract()
        # tagList = [element for element in tagList if not element.strip().endswith("评论")]
        # ret_tags = ",".join(tagList)

        # 设定jobboleItem
        # article_item['title'] = ret_title
        # article_item['create_date'] = ret_create_date
        # article_item['url'] = response.url
        # article_item['url_object_id'] = get_md5(response.url)
        # article_item['front_image_url'] = [front_image_url]
        # article_item['praise_nums'] = ret_praise_nums
        # article_item['comment_nums'] = ret_comment_nums
        # article_item['fav_nums'] = ret_fav_nums
        # article_item['tags'] = ret_tags
        # article_item['content'] = ret_content
        # yield article_item

        item_loader = ArticleItemLoader(item=JobBoleArticleItem(), response=response)
        item_loader.add_xpath("title", '//div[@class="entry-header"]/h1/text()')
        item_loader.add_xpath("create_date", "//p[@class='entry-meta-hide-on-mobile']/text()")
        item_loader.add_xpath("praise_nums", "//span[contains(@class, 'vote-post-up')]/h10/text()")
        item_loader.add_xpath("fav_nums", "//span[contains(@class, 'bookmark-btn')]/text()")
        item_loader.add_xpath("comment_nums", "//a[@href='#article-comment']/span/text()")
        item_loader.add_xpath("tags", "//p[@class='entry-meta-hide-on-mobile']/a/text()")
        item_loader.add_xpath("content", "//div[@class='entry']/text()")
        item_loader.add_value("url", response.url)
        item_loader.add_value("url_object_id", get_md5(response.url))
        item_loader.add_value("front_image_url", [front_image_url])

        article_item = item_loader.load_item()
        yield article_item
        pass

```

`items.py`

``` python
# -*- coding: utf-8 -*-

# Define here the models for your scraped items
#
# See documentation in:
# http://doc.scrapy.org/en/latest/topics/items.html

import datetime
import re
import scrapy
from scrapy.loader.processors import MapCompose,TakeFirst,Join
from scrapy.loader import ItemLoader


class ArticlespiderItem(scrapy.Item):
    # define the fields for your item here like:
    # name = scrapy.Field()
    pass

def add_jobbole(value):
    return value + "-jobbole"

def convert_date(value):
    try:
        create_date = datetime.datetime.strptime(value, '%Y/%m/%d').date()
    except Exception as e:
        create_date = datetime.datetime.now().date()
    return create_date

def get_nums(value):
    reHandle = re.match(".*(\d+).*", value)
    if reHandle:
        ret_comment_nums = int(reHandle.group(1))
    else:
        ret_comment_nums = 0
    return ret_comment_nums

def remove_comment_tags(value):
    if "评论" in value:
        return ""
    else:
        return value

def return_value(value):
    return value

class ArticleItemLoader(ItemLoader):
    default_output_processor = TakeFirst()

class JobBoleArticleItem(scrapy.Item):
    title = scrapy.Field(
        input_processor = MapCompose(
            add_jobbole,
            lambda x:x+"-AGAIN"
        )
    )
    create_date = scrapy.Field(
        input_processor = MapCompose(
            convert_date
        ),
        output_processor = TakeFirst()
    )
    url = scrapy.Field()
    # url md5
    url_object_id = scrapy.Field()
    # 封面图
    front_image_url = scrapy.Field(
        output_processor = MapCompose(return_value)
    )
    # 封面图本地下载后路径
    front_image_path = scrapy.Field()
    # 点赞
    praise_nums = scrapy.Field(
        input_processor = MapCompose(
            get_nums
        )
    )
    # 评论
    comment_nums = scrapy.Field(
        input_processor = MapCompose(
            get_nums
        )
    )
    # 收藏
    fav_nums = scrapy.Field(
        input_processor = MapCompose(
            get_nums
        )
    )
    # 标签
    tags = scrapy.Field(
        # input_processor = MapCompose(remove_comment_tags),
        output_processor = Join(",")
    )
    content = scrapy.Field()
    pass
```

可以加工`title`做一些转化操作，必须引入`MapCompose`类指定加载顺序

`output_processor`指定了输出属性的提取规则，通过scrapy提供的`TakeFirst()`我们可以默认提取list中的第一个元素

另外，scrapy同时也提供了`Join`方法来默认支持拼接

我们要为每个属性都指定output_processor吗，当然不！因为这样太大的代码修改了，不符合我们软件工程中的复用的思想，这里我们通过修改默认的output_processor的行为来搞定

``` python
from scrapy.loader import ItemLoader
class ArticleItemLoader(ItemLoader):
    default_output_processor = TakeFirst()
```


## 知识点
- 3个提取指定方法
    - item_loader.add_css()
    - item_loader.add_xpath()
    - item_loader.add_value()
- 匿名函数
    - `lambda x:x+'-demo'`
