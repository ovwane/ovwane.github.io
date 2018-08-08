---
date: 2017-06-23 11:22:02
---

```

scrapy startproject crawl_spider
cd crawl_spider

scrapy genspider jobbole "blog.jobbole.com"
scrapy genspider zhihu "www.zhihu.com"
scrapy genspider lagou "www.lagou.com"
```

pycharm调试scrapy

main.py 和 scrapy.cfg 在同一级目录下

```py
#!/usr/bin/env python
# -*- coding:utf-8 -*-
from scrapy.cmdline import execute

import sys
import os


sys.path.append(os.path.dirname(os.path.abspath(__file__)))
execute(["scrapy","crawl","jobbole"])
```

settings.py

```py
ROBOTSTXT_OBEY = False
```

伯乐在线提取文章
标题，日期，评论，点赞，

xpath

xpath语法
/div/* 选取属于div元素的所有子节点
//* 选取所有元素
//div[@*] 选取所有带属性的div元素
/div/a|//div/p 选取所有div元素的a和p元素
//span|//ul 选取文档中的span和ul元素
article/div/p|//span 选取所有属于artile的div的p元素和所有的span元素


scrapy shell 调试 

```bash
scrapy shell http://blog.jobbole.com/110287/
```

```py
title = response.xpath("")

title = response.xpath("//div[@class="entry-header"]/h1")

title = response.xpath("//div[@class="entry-header"]/h1/text()")

title = response.xpath("//div[@class="entry-header"]/h1/text()").extract()[0]

response.xpath('//div[@id="archive"]//a[@class="archive-title"]/@href').extract()
```

```py
[element for element in tag_list if not element.strip().endswith("评论")]
```

css selector

css选择器

```
* 选择所有节点
#con 选择id为con的节点
.con 选择所有class包含con的节点
li a 选取所有li下的所有a节点
ul + p 选择ul后面的第一个p元素
div#con > ul 选取id为con的div的第一个ul子元素
ul ~ p 选择与ul相邻的所有p元素
a[title] 选取所有有tilte属性的a元素
input[type=radio]:checked 选择选中的radio的元素
div:not(#con) 选取所有id非con的div属性
li:nth-child(3)选取第三个li元素
tr:nth-child(2n)第偶数个tr
```

```py
title = response.css("")

title = response.css(".entry-header h1")

title = response.css(".entry-header h1::text")

title = response.css(".entry-header h1:text").extract()
```

1.获取文章列表页的文章url并交给scrapy下载后并进行解析
2.获取下一页的url并交给scrapy进行下载，下载完成后交给parse

```py
post_urls = response.css(".con a::attr(href)").extract()
```

```py
from scrapy.http import Request
from urllib import parse

#下载所有文章的url
yield Request(url=parse.urljoin(response.url, post_url),callback=self.parse_detail)

#提取下一页的url
next_urls = response.css(".next.page-number::attr(href)").extract()
if next_url:
	yield Request(url=parse.urljoin(response.url, post_url),callback=self.parse) 
```

items.py

```py
class JobboleItem(scrapy.Item):
	title = scrapy.Field()
```

scrapy自带的下载中间件
图片中间件

```py
BASE_DIR = os.path.dirname(os.path.abspath(__file__))
IMAGES_URLS_FIELD = ""
IMAGES_STORE = os.path.join(BASE_DIR, 'images')
```

1.先定义items
scrapy items
2.在jobbole.py爬虫写处理逻辑
yield 爬去的数据
3.在pipelines中定义怎么储存yield过来的数据
3.1 设置setttings.py中的downloader中间件
```
DOWNLOADER_MIDDLEWARES = {   'crawl_spider.middlewares.MyCustomDownloaderMiddleware': 543,
}
```

item loader机制


