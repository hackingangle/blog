---
title: scrapy-css选择器
date: 2017-07-11 09:10:39
tags: scrapy
---

# what

css定义了html中的样式，是一个网页中必不可少的元素。

css选择器用来定位某一个html节点，与`xpath`作用相同。

## 语法

- *
    - 选择所有节点
- #container
    - id为container节点
- .container
    - class包含container节点
- li a
    - 所有li下所有a节点
- ul + p
    - ul后第一个p
- div#cotainer > ul
    - id为container节点的div的第一个ul子元素
- ul ~ p
    - 与ul相邻的所有p元素
- a[title]
    - 有title属性a元素
- a[href="http://jobbole.com"]
    - href为http://jobbole.com的a元素
- a[href*="jobbole"]
    - href包含jobbole的a元素
- a[href^="http"]
    - 以http开头的a元素
- a[href$=".jpg"]
    - 以.jpg结尾的a元素
- input[type=radio]:checked
    - 选择的radio元素
- div:not(#container)
    - id不等于container的div元素
- li:nth-child(3)
    - 第三个li元素
- tr:nth-child(2n)
    - 偶数tr

# how

## scrapy shell

进入shell模式
``` shell
scrapy shell http://blog.jobbole.com/111742/
```

交互下调试:

``` python
# 标题
ret_title = response.css(".entry-header h1::text").extract()[0]
# 创建时间
ret_create_date = response.css("p.entry-meta-hide-on-mobile::text").extract()[0].strip().replace(" ·",  "")

```

- 知识点
    - extract_first("")
        - 如果list第一个元素为空，则返回""
