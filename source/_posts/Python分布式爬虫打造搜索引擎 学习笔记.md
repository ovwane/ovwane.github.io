# [Python分布式爬虫打造搜索引擎](http://coding.imooc.com/class/92.html)

## 第1章 课程介绍
#### 介绍课程目标、通过课程能学习到的内容、和系统开发前需要具备的知识

```
Scrapy 
elasticsearch
django
```

## 第2章 windows下搭建开发环境
#### 介绍项目开发需要安装的开发软件、 python虚拟virtualenv和 virtualenvwrapper的安装和使用、 最后介绍pycharm和navicat的简单使用

```
PyCharm安装和配置
MySQL和Navicat的安装使用
CentOS安装Python2和Python3
Python虚拟环境的安装配置virtualenv、virtualenvwrapper
```

## 第3章 爬虫基础知识回顾
#### 介绍爬虫开发中需要用到的基础知识包括爬虫能做什么，正则表达式，深度优先和广度优先的算法及实现、爬虫url去重的策略、彻底弄清楚unicode和utf8编码的区别和应用。

##### 技术选型
```
scrapy vs requests+beautifulsoup
1.requests和beautifulsoup都是库，scrapy是框架
2.scrapy框架中可以加入requests和beautifulsoup
3.scarpy基于twisted，性能是最大的优势
4.scarpy方便扩展，提供了很多内置的功能
5.scrapy内置的css和xpath selector非常方便，beautifulsoup最大的缺点就是慢
```

##### 网页分类
```
常见类型的服务
1.静态网页
2.动态网页
3.webservice(restapi)
```

##### 爬虫能做什么
```
爬虫作用
1.搜索引擎---百度、google、垂直领域搜索引擎
2.推荐引擎---今日头条
3.机器学习的数据样本
4.数据分析（如金融数据分析）、舆情分析等
```

#### 正则表达式
```
目录
1.特殊字符
1)
```
```python
import re

# 待匹配字符串
line = 'ssfder'
# 匹配规则
regex_str = "^b.*"
# 匹配字符
if re.match(regex_str, line):
	print("yes")
```

#### 字符串编码
```
1.ASCII
2.Unicode
3.可变长编码 utf-8
```

```flow
文件：test.txt utf-8编码--->(read)读取：转换为unicode编码--->(read)内存中 Unicode编码--->(save)保存：转换为utf-8编码
```

#### 爬虫去重策略
```
1.将访问过的url保存到数据库中
2.将访问过的url保存到set中，只需要o(1)的代价就可以查询url 100000000*2byte*50个字符/1024/1024/1024 = 9GB 
3.url经过md5等方法哈希后保存到set中
4.用bitmap方法，将访问过的url通过hash函数映射到某一位
5.bloomfilter方法对bitmap进行改进，多重hash函数降低冲突
```

#### 深度优先和广度优先
```
目录
1.网站的树结构
2.深度优先算法和实现
递归实现
3.广度优先算法和实现
队列实现
```

## 第4章 scrapy爬取知名技术文章网站
#### 搭建scrapy的开发环境，本章介绍scrapy的常用命令以及工程目录结构分析，本章中也会详细的讲解xpath和css选择器的使用。然后通过scrapy提供的spider完成所有文章的爬取。然后详细讲解item以及item loader方式完成具体字段的提取后使用scrapy提供的pipeline分别将数据保存到json文件以及mysql数据库中。...
#### xpath
##### xpath简介
```
1.xpath使用路径表达式在xml和html中进行导航
2.xpath包含标准函数库
3.xpath是一个w3c的标准
```

##### xpath节点关系
```
1.父节点
2.子节点
3.同胞节点
4.先辈节点
5.
```
##### xpath语法
```
表达式 说明
article 选取所有article元素的所有子节点
/article 选取根元素article
article/a 选取所有属于article的子元素的a元素
//div 选取所有div子元素（不论出现在文档任何地方）
article//div 选取所有属于article元素的后代的div元素，不管它出现在article之下的任何位置
//@class 选取所有名为class的属性

/div/* 选取属于div元素的所有子节点
//* 选取所有元素
//div[@*] 选取所有带属性的title元素
//div/a|//div/p 选取所有div元素的a和p元素
//span|//ul 选取文档中的span和ul元素
article/div/p|//span 选取所有属于article元素的div元素的p元素 以及文档中所有的span元素
```

##### xpath语法-谓语
```
表达式 说明
/article/div[1] 选取属于article子元素的第一个div元素
/article/div[last()] 选取属于article子元素的最后一个div元素
/article/div[last()-1] 选取属于article子元素的倒数第二个div元素
//div[@lang] 选取所有拥有lang属性的div元素
//div[@lang='eng'] 选取所有lang属性为eng的div元素
```

##### scrapy shell
```
scrapy shell http://blog.jobbole.com/110287

title = response.xpath("//div[@class='entry-header']/h1/text()")

title

title.extract()
```

#### css选择器
```
表达式 说明
* 选择所有节点
#container 选择id为container的节点
.container 选择所有class包含container的节点
li a 选择所有li下的所有a节点
ul + p 选择ul后面的第一个p元素
div#container > ul 选取id为container的div的第一个ul子元素
```
```
表达式 说明
ul ～ p 选取与ul相邻的所有p元素
a[title] 选取所有有title属性的a元素
a[href="http://jobbole.com"] 选取所有href属性为jobbole.com值的a元素
a[href*="jobbole"] 选取所有href属性包含jobbole的a元素
a[href^="http"] 选取所有href属性值以http开头的a元素
a[href$=".jpg"] 选取所有href属性值以.jpg结尾的a元素
input[type=radio]:checked 选择选中的radio的元素
```

```
表达式 说明
div:not(#container) 选取所有id非container的div属性
li:nth-child(3) 选取第三个li元素
tr:nth-child(2n) 第偶数个tr
```

##### scrapy shell
```
scrapy shell http://blog.jobbole.com/110287

title = response.css(".entry-header h1::text")

title

title.extract()
```

#### scrapy.http
```
from scrapy.http import Request
```

#### scrapy-djangoitem

#### item-loader jobbole.py 
```python
from scrapy.loader import ItemLoader
# 通过item_loader加载item,JobboleArticleItem()是item.py里的item。
item_loader = ItemLoader(item=JobboleArticleItem(), response=response)
item_loader.add_css("title", ".entry-header h1::text")
item_loader.add_xpath()
item_loader.add_value("url", response.url)
# 重新加载
article_item = item_loader.load_item()
# 传出
yield article_item
```

#### item.py
```
class JobBoleArticleItem(scrapy.Item):
    title = scrapy.Field()
    create_date = scrapy.Field(
        input_processor=MapCompose(date_convert),
    )
    content = scrapy.Field()
```

## 第5章 scrapy爬取知名问答网站
***

本章主要完成网站的问题和回答的提取。本章除了分析出问答网站的网络请求以外还会分别通过requests和scrapy的FormRequest两种方式完成网站的模拟登录， 本章详细的分析了网站的网络请求并分别分析出了网站问题回答的api请求接口并将数据提取出来后保存到mysql中。
***

#### cookie
```
distributed_crawler_search_engine
```
```
scrapy shell -s "输入headers" 
```
知乎数据库设计

```
名 类型
zhihu_id bigint 20 0 yes primay key 1
url varchar 300 0 yes
question_id bigint 20 0 yes
author_id varchar 100 0 no
content longtext 0 0 yes
praise_num int 11 0 yes
comments_num int 11 0 yes
create_time date 0 0 yes
update_time date 0 0 yes
crawl_time datetime 0 0 yes
crawl_update_time datetime 0 0 no
```

```sql
CREATE TABLE zhihu_question(`zhihu_id` 
 )
```

## 第6章 通过CrawlSpider对招聘网站进行整站爬取
#### 本章完成招聘网站职位的数据表结构设计，并通过link extractor和rule的形式并配置CrawlSpider完成招聘网站所有职位的爬取，本章也会从源码的角度来分析CrawlSpider让大家对CrawlSpider有深入的理解。

## 第7章 Scrapy突破反爬虫的限制
>本章会从爬虫和反爬虫的斗争过程开始讲解，然后讲解scrapy的原理，然后通过随机切换user-agent和设置scrapy的ip代理的方式完成突破反爬虫的各种限制。本章也会详细介绍httpresponse和httprequest来详细的分析scrapy的功能，最后会通过云打码平台来完成在线验证码识别以及禁用cookie和访问频率来降低爬虫被屏蔽的可能性。...

### UA 用户代理
hellysmile/fake-useragent

### IP代理
西刺代理 高匿ip代理
aivarsk/scrapy-proxies
scrapy-plugins/scrapy-crawlera #收费的
Tor

### 验证码识别
tesseract-ocr
在线打码
人工打码
[云打码](http://www.yundama.com)

settings.py
禁用cookie
自动限速

爬虫内写入 ，自定义设置单个爬虫
```py
custom_settings = {
	"COOKIES_ENABLED": True
}
```

## 第8章 scrapy进阶开发
>本章将讲解scrapy的更多高级特性，这些高级特性包括通过selenium和phantomjs实现动态网站数据的爬取以及将这二者集成到scrapy中、scrapy信号、自定义中间件、暂停和启动scrapy爬虫、scrapy的核心api、scrapy的telnet、scrapy的web service和scrapy的log配置和email发送等。 这些特性使得我们不仅只是可以通过scrapy来完成...

Selenium浏览器自动化测试框架
浏览器Drivers


## 第9章 scrapy-redis分布式爬虫
#### Scrapy-redis分布式爬虫的使用以及scrapy-redis的分布式爬虫的源码分析， 让大家可以根据自己的需求来修改源码以满足自己的需求。最后也会讲解如何将bloomfilter集成到scrapy-redis中。

## 第10章 elasticsearch搜索引擎的使用
#### 本章将讲解elasticsearch的安装和使用，将讲解elasticsearch的基本概念的介绍以及api的使用。本章也会讲解搜索引擎的原理并讲解elasticsearch-dsl的使用，最后讲解如何通过scrapy的pipeline将数据保存到elasticsearch中。

## 第11章 课程总结
#### 本章讲解如何通过django快速搭建搜索网站， 本章也会讲解如何完成django与elasticsearch的搜索查询交互。

## 第12章 scrapyd部署scrapy爬虫
#### 本章主要通过scrapyd完成对scrapy爬虫的线上部署。

## 第13章 课程总结
#### 重新梳理一遍系统开发的整个过程， 让同学对系统和开发过程有一个更加直观的理解

```
pip3 install selenium redis elasticsearch_dsl PyMySQL requests
```
