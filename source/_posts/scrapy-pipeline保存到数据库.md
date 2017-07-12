---
title: scrapy-pipeline保存到数据库
date: 2017-07-12 07:26:56
tags: scrapy
---

我们介绍了python环境依赖管理virtual安装、scrapy安装、xpath、css提取方法、items、pipeline、ImagePipeline自定义，
通过上述的知识，我们可以把网页中的信息提取出来，组装成items，然后把items流通在一个个数据管道里面，今天我们说的就是发生在
某一个数据管道里面的事情，把items数据做持久化处理：

1. 保存本地json文件
1. 保存到mysql数据库表中

## 保存本地jsonhow

### 自定义JsonPipeline

``` python
class JsonWithEncodingPipeline(object):
    def __init__(self):
        self.file = codecs.open("articles.json", "w", encoding="utf-8")
    def process_item(self, item, spider):
        lines = json.dumps(dict(item), ensure_ascii=False)
        self.file.write(lines)
        return item
    def spider_closed(self, spider):
        self.file.close()
```

`ensure_ascii=False`保证utf8字符不乱码

`settings.py`

``` python
ITEM_PIPELINES = {
   'ArticleSpider.pipelines.JsonWithEncodingPipeline': 2,
   'ArticleSpider.pipelines.ArticleImagePipeline': 1,
   # 'scrapy.pipelines.images.ImagesPipeline': 11,
}
```

### scrapy.exporters
``` python
from scrapy.exporters import JsonItemExporter
class JsonExporterPipeline(object):
    def __init__(self):
        self.file = open('articleexporter.json', 'wb')
        self.exporter = JsonItemExporter(self.file, encoding="utf8", ensure_ascii=False)
        self.exporter.start_exporting()

    def close_spider(self, spider):
        self.exporter.finish_exporting()
        self.file.close()

    def process_item(self, item, spider):
        self.exporter.export_item(item)
        return item
```

`ensure_ascii=False`保证utf8字符不乱码

`settings.py`

``` python
ITEM_PIPELINES = {
   'ArticleSpider.pipelines.JsonExporterPipeline': 2,
   'ArticleSpider.pipelines.ArticleImagePipeline': 1,
   # 'scrapy.pipelines.images.ImagesPipeline': 11,
}
```

### mysql保存

#### 设计数据库表

laravel 中 migrations:

``` php
<?php

use Illuminate\Support\Facades\Schema;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateArticlesTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('articles', function (Blueprint $table) {
            $table->string('title', 200);
            $table->date('create_date')->nullable();
            $table->string('url', 300);
            $table->string('url_object_id', 50);
            $table->string('front_image_url', 300)->nullable();
            $table->string('front_image_path', 200)->nullable();
            $table->integer('comment_nums')->nullable();
            $table->integer('fav_nums')->nullable();
            $table->integer('praise_nums')->nullable();
            $table->string('tags', 200)->nullable();
            $table->longText('content')->nullable();
            $table->primary('url_object_id');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('articles');
    }
}

```

#### 安装mysql驱动
``` shell
workon scrapy_python3
pip install mysqlclient
```

ubuntu下报错，需要安装
``` shell
sudo apt-get install libmysqlclient-dev
```

centos下报错，需要安装
``` shell
sudo yum install python-devel mysql-devel
```

#### 同步插入
``` python
import MySQLdb
class MysqlPipeline(object):
    def __init__(self):
        host = "127.0.0.1"
        user = "homestead"
        password = "secret"
        dbname = "jobbole"
        port = 33060
        self.conn = MySQLdb.connect(host, user, password, dbname, port=port, charset="utf8", use_unicode=True)
        self.cursor = self.conn.cursor()

    def process_item(self, item, spider):
        sql = """
            insert into articles(title, url, url_object_id, create_date, fav_nums)
            VALUES (%s, %s, %s, %s, %s)
        """
        self.cursor.execute(sql, (item["title"], item["url"], item["url_object_id"], item["create_date"], item["fav_nums"]))
        self.conn.commit()
        return item
```

`settings.py`
``` python
ITEM_PIPELINES = {
   'ArticleSpider.pipelines.MysqlPipeline': 2,
   'ArticleSpider.pipelines.ArticleImagePipeline': 1,
   # 'scrapy.pipelines.images.ImagesPipeline': 11,
}
```

#### 异步插入

``` python
import MySQLdb
import MySQLdb.cursors
# twisted提供了异步容器，底层还是MYSQLdb
class MysqlTwistedPipeline(object):
    def __init__(self, dbpool):
        self.dbpool = dbpool

    @classmethod
    # 被scrapy调用的hook，传递settings.py值进入
    def from_settings(cls, settings):
        dbparams = dict(
            host = settings["MYSQL_HOST"],
            user = settings["MYSQL_USER"],
            password = settings["MYSQL_PASSWORD"],
            db = settings["MYSQL_DBNAME"],
            port = settings["MYSQL_PORT"],
            charset = "utf8",
            cursorclass = MySQLdb.cursors.DictCursor,
            use_unicode = True,
        )
        dbpool = adbapi.ConnectionPool("MySQLdb", **dbparams)
        return cls(dbpool)

    # 异步操作异常需要单独处理
    def process_item(self, item, spider):
        query = self.dbpool.runInteraction(self.doMysqlTwistedPipeline, item)
        # 处理异常
        query.addErrback(self.handle_errors, item, spider)

    def handle_errors(self, failure, item, spider):
        print("------------------------------error==========================>", failure)

    def doMysqlTwistedPipeline(self, cursor, item):
        sql = """
                    insert into articles(title, url, url_object_id, create_date, fav_nums)
                    VALUES (%s, %s, %s, %s, %s)
                """
        cursor.execute(sql, (item["title"], item["url"], item["url_object_id"], item["create_date"], item["fav_nums"]))
```

`settings.py`中

``` python
ITEM_PIPELINES = {
   'ArticleSpider.pipelines.MysqlTwistedPipeline': 2,
   'ArticleSpider.pipelines.ArticleImagePipeline': 1,
   # 'scrapy.pipelines.images.ImagesPipeline': 11,
}
# mysql confs
MYSQL_HOST = "127.0.0.1"
MYSQL_USER = "homestead"
MYSQL_PASSWORD = "secret"
MYSQL_DBNAME = "jobbole"
MYSQL_PORT = 33060
```

## 知识点

1. 打开文件
``` python
import codecs
file = codecs.open('article.json', 'w', encoding='utf-8')
file.write(lines)
```

1. items转dict转json
``` python
import json
json.dumps(dict(items), ensure_ascii=False)
```

1. 识别spider关闭事件来处理关闭连接
``` python
def spider_closed(self, spider):
    self.file.close()
```

1. 二进制方式打开文件
``` python
file = open("xx.json", "wb")
```

1. 日期转换
``` python
import datetime
try:
    create_date = datetime.datetime.strptime(create_date, "%Y/%m/%d").date()
except Exception as e:
    create_date = datetime.datetime.now().date() 
```

1. md5后字符长度为`50`
1. 可变化参数
    - `**dbparams`

