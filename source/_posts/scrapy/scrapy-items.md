---
title: scrapy-items&pipelines
date: 2017-07-11 21:36:32
tags: scrapy
---

# what

数据库dao对象，可以不指定类别定义对象属性，为了保存到数据库提供便利

## 定义

在`items.py`中
``` python
class JobBoleArticleItem(scrapy.Item):
    title = scrapy.Field()
    create_date = scrapy.Field()
    url = scrapy.Field()
    # url md5
    url_object_id = scrapy.Field()
    # 封面图
    front_image_url = scrapy.Field()
    # 封面图本地下载后路径
    front_image_path = scrapy.Field()
    # 点赞
    praise_nums = scrapy.Field()
    # 评论
    comment_nums = scrapy.Field()
    # 收藏
    fav_nums = scrapy.Field()
    # 标签
    tags = scrapy.Field()
    content = scrapy.Field()
    pass
```

## 使用

在爬虫程序中：
``` python
from ArticleSpider.items import JobBoleArticleItem

# 实例化JobBoleArticleItem
article_item = JobBoleArticleItem()

# 设定jobboleItem
article_item['title'] = ret_title
article_item['create_date'] = ret_create_date
article_item['url'] = response.url
article_item['front_image_url'] = front_image_url
article_item['praise_nums'] = ret_praise_nums
article_item['comment_nums'] = ret_comment_nums
article_item['fav_nums'] = ret_fav_nums
article_item['tags'] = ret_tags
article_item['content'] = ret_content

# 交付pipeline
yield article_item
```

`yield article_item`这里是重点中的重点，再来回顾一下：
- 如果`yield Request`把一个url交付到Scrapy中下载分析；
- 如果`yield XX_item`是把item交付到pipeline中

## pipelines

pipelines收到yield的`items`，然后处理，如果要开启pipelines，也需要设置

在`settings.py`

``` python
# 开启注释
ITEM_PIPELINES = {
   'ArticleSpider.pipelines.ArticlespiderPipeline': 300,
}
```
这时，我们的默认的一个pipeline`ArticlespiderPipeline`就被激活了，断点可以打到这里


![pipeline调试](/images/scrapy/pipeline调试.jpeg)

### pipelines之自动下载图片

pipelines是各种功能管道，用来处理数据对象items的各个属性，接下来，我们来分析一种下载图片的pipeline

`settings.py`开启设置：

``` python
import os
# 开启注释
ITEM_PIPELINES = {
   'ArticleSpider.pipelines.ArticlespiderPipeline': 300,
   'scrapy.pipelines.images.ImagesPipeline': 1,
}
# 图片url字段，位于item中
IMAGES_URLS_FIELD = "front_image_url"
# 项目目录
project_dir = os.path.dirname(os.path.abspath(__file__))
# 图片存储的目录
IMAGES_STORE = os.path.join(project_dir, "images")
# 图片最小宽度
IMAGES_MIN_WIDTH = 100
# 图片最小高度
IMAGES_MIN_HEIGHT = 100
```

执行后，发现缺少库`PIL`，则切到环境安装：

``` shell
workon scrapy_pyth
pip install pillow
```

还是执行不了，提示异常
``` shell
Traceback (most recent call last):
  File "/Users/wanggang/.virtualenvs/scrapy_python3/lib/python3.6/site-packages/twisted/internet/defer.py", line 653, in _runCallbacks
    current.result = callback(current.result, *args, **kw)
  File "/Users/wanggang/.virtualenvs/scrapy_python3/lib/python3.6/site-packages/scrapy/pipelines/media.py", line 79, in process_item
    requests = arg_to_iter(self.get_media_requests(item, info))
  File "/Users/wanggang/.virtualenvs/scrapy_python3/lib/python3.6/site-packages/scrapy/pipelines/images.py", line 152, in get_media_requests
    return [Request(x) for x in item.get(self.images_urls_field, [])]
  File "/Users/wanggang/.virtualenvs/scrapy_python3/lib/python3.6/site-packages/scrapy/pipelines/images.py", line 152, in <listcomp>
    return [Request(x) for x in item.get(self.images_urls_field, [])]
  File "/Users/wanggang/.virtualenvs/scrapy_python3/lib/python3.6/site-packages/scrapy/http/request/__init__.py", line 25, in __init__
    self._set_url(url)
  File "/Users/wanggang/.virtualenvs/scrapy_python3/lib/python3.6/site-packages/scrapy/http/request/__init__.py", line 58, in _set_url
    raise ValueError('Missing scheme in request url: %s' % self._url)
ValueError: Missing scheme in request url: h
```

为什么呢？看异常问题出在处理图片下载的pipeline上面，我们可以看到在`settings.py`中设置了`front_image_url`属性为
items中的下载图片的属性，但是有一个问题，默认的ImagesPipeline肯定不想只是下载一个图，所以他默认会把这个属性当成`[]`处理，
所以我们在初始化的时候需要做如下：
``` python
article_item['front_image_url'] = [front_image_url]
```

![imagesPipelines成功下图](/images/scrapy/imagesPipelines成功下图.jpeg)

## 定制化ImagePipeline

太赞了，我们通过ImagePipeline已经可以实现图片的自动下载了，但是，但是，我想说的是但是，但是不要骄傲

因为本地路径还没有保存到item中呢，我们如何实现？

ImagePipeline封装在scrapy提供的第三方包中，我们一定不能直接修改源代码，要通过继承的方式拓展开来

这样，我们就再定一个Pipeline来实现本地图片下载路径写入到item中的需求吧

`pipelines.py`创建新的`ArticleImagePipeline`

```
class ArticleImagePipeline(ImagesPipeline):
    def item_completed(self, results, item, info):
        for ok, value in results:
            image_file_path = value["path"]
        item["front_image_path"] = image_file_path
        return item
```

修改优先级`settings.py`

``` python
ITEM_PIPELINES = {
   'ArticleSpider.pipelines.ArticlespiderPipeline': 2,
   'ArticleSpider.pipelines.ArticleImagePipeline': 1,
   # 'scrapy.pipelines.images.ImagesPipeline': 11,
}
```

## md5

python中md5如何计算，需要自己定义！

创建一个工具package:utils

``` python
import hashlib

def get_md5(url):
    m = hashlib.md5()
    m.update(url)
    return m.hexdigest()

if __name__ == "__main__":
    print(get_md5("http://www.baidu.com"))
```

执行报错如下：
``` shell
/Users/wanggang/.virtualenvs/scrapy_python3/bin/python3.6 /Users/wanggang/scrapy/ArticleSpider/ArticleSpider/utils/common.py
Traceback (most recent call last):
  File "/Users/wanggang/scrapy/ArticleSpider/ArticleSpider/utils/common.py", line 9, in <module>
    print(get_md5("http://www.baidu.com"))
  File "/Users/wanggang/scrapy/ArticleSpider/ArticleSpider/utils/common.py", line 5, in get_md5
    m.update(url)
TypeError: Unicode-objects must be encoded before hashing

Process finished with exit code 1
```

报错原因很简单，因为在python3中，所有的字符串自动认为是unicode的，所以，我们需要转utf8

``` python
"http://www.baidu.com".encode("utf-8")
```

当然，我们可以让程序更健壮些：

``` python
import hashlib

def get_md5(url):
    if isinstance(url, str):
        url = url.encode("utf-8")
    m = hashlib.md5()
    m.update(url)
    return m.hexdigest()

if __name__ == "__main__":
    print(get_md5("http://www.baidu.com"))
```

# 知识点
1. `Request`可以携带数据到下一个请求的结果对象中
    - 参数代入数据
        - `Request(meta={"front_image_url":front_image_url})`
    - 使用
        - 可能溢出
            - `response.meta["front_image_url"]`
        - 防溢出
            - `response.meta.get("front_image_url", "")`
1. os.path.dirname
1. os.path.join(project_dir, "images")
1. os.path.abspath(__file__)
1. __file__
1. scrapy.pipelines.images.ImagesPipeline
    - IMAGES_URLS_FIELD
        - 图片url字段，位于item中
    - IMAGES_STORE
        - 图片存储的目录
1. tuple解包
    - [(True, 'http://www.baidu.com')]
        - `for ok, value in results:`
