---
title: Python分布式爬虫打造搜索引擎-学习笔记
date: 2017-05-01 20:18:02
---

第1章 课程介绍
第2章 Windows下搭建开发环境
第3章 爬虫基础知识回顾
第4章 Scrapy爬取知名技术文章网站
第5章 Scrapy爬取知名问答网站
第6章 通过CrawlSpider对招聘网站进行整站爬取
第7章 Scrapy突破反爬虫的限制
第8章 Scrapy进阶开发
第9章 Scrapy-redis分布式爬虫
第10章 Elasticsearch搜索引擎的使用
第11章 Django搭建搜索网站
第12章 Scrapyd部署Scrapy爬虫
第13章 课程总结



# 第1章 课程介绍

1-1 python分布式爬虫打造搜索引擎简介.mp4 07:31

# 第2章 Windows下搭建开发环境
2-1 pycharm的安装和简单使用.mp4 10:28
2-2 mysql和navicat的安装和使用.mp4 16:21
2-3 windows和linux下安装python2和python3.mp4 06:49
2-4 虚拟环境的安装和配置.mp4 30:53

# 第3章 爬虫基础知识回顾
3-1 技术选型 爬虫能做什么(1-2)(第2节09分钟58秒).mp4 28:37
3-3 正则表达式-2(3-4)(第4节16分钟00秒).mp4 32:23
3-5 深度优先和广度优先原理.mp4 25:27
3-6 url去重方法(7-6)(第6节15分钟20秒).mp4 23:28

# 第4章 Scrapy爬取知名技术文章网站
4-1 scrapy安装以及目录结构介绍(1-3).mp4 57:25
4-4 xpath的用法 - 2(4-5).mp4 32:41
4-6 css选择器实现字段解析 - 1(6-7).mp4 27:23
4-7 css选择器实现字段解析 - 2(7-15).mp4 2:24:38
4-8 编写spider爬取jobbole的所有文章 - 1(8-9) .mp4 20:38
4-16 scrapy item loader机制 - 1.mp4 17:26
4-17 scrapy item loader机制- 2.mp4 31:28

# 第5章 Scrapy爬取知名问答网站
5-1 session和cookie自动登录机制(1-5).mp4 1:20:08
5-6 知乎分析以及数据表设计1(6-16).mp4 2:46:21
5-17 (补充小节)知乎验证码登录 - 2_1.mp4 10:32

# 第6章 通过CrawlSpider对招聘网站进行整站爬取
6-1 数据表结构设计(1-4).mp4 1:08:21
6-5 item loader方式解析职位(5-7) 7-1(1).mp4 1:15:24

# 第7章 Scrapy突破反爬虫的限制
7-2 scrapy架构源码分析(2-10).mp4 2:18:32

# 第8章 Scrapy进阶开发
8-1 selenium动态网页请求与模拟登录知乎.mp4 21:30
8-2 selenium模拟登录微博， 模拟鼠标下拉.mp4 11:08
8-3 chromedriver不加载图片、phantomjs获取动态网页.mp4 10:04
8-4 selenium集成到scrapy中.mp4 16:30
8-5 其余动态网页获取技术介绍-chrome无界面运行、scrapy-splash、selenium-grid, splinter.mp4 08:01
8-6 scrapy的暂停与重启.mp4 10:17
8-7 scrapy url去重原理.mp4 05:52
8-8 scrapy telnet服务.mp4 07:00
8-9 spider middleware 详解.mp4 10:46
8-10 scrapy的数据收集.mp4 10:40
8-12 scrapy扩展开发.mp4 11:52

# 第9章 Scrapy-redis分布式爬虫
9-1 分布式爬虫要点.mp4 08:59
9-2 redis基础知识 - 1.mp4 16:29
9-3 redis基础知识 - 2.mp4 20:37
9-5 scrapy源码解析-connection.py、defaults.py-.mp4 05:51
9-6 scrapy-redis源码剖析-dupefilter.py-.mp4 05:57
9-7 scrapy-redis源码剖析- pipelines.py、 queue.py-.mp4 11:19

# 第10章 Elasticsearch搜索引擎的使用
10-1 elasticsearch介绍.mp4 16:34
10-2安装.mp4 13:40
10-3.mp4 26:45
10-4.mp4 11:34
10-5倒排索引.mp4 11:06
10-6.mp4 19:00
10-7.mp4 12:47
10-8.mp4 21:04
10-9.mp4 15:00
10-10.mp4 11:15
10-11.mp4 23:00              
10-12.mp4 14:18                  
10-13.mp4 11:18

# 第11章 Django搭建搜索网站
11-1 es完成搜索建议.mp4 14:00
11-2.mp4 14:09
11-3.mp4 19:52                    
11-4.mp4 18:45
11-5.mp4 13:01
11-6.mp4 13:19
11-7.mp4 09:18

# 第12章 Scrapyd部署Scrapy爬虫
12-1 scrapyd部署scrapy项目.mp4 19:10

# 第13章 课程总结





ArticleSpider运行环境

```shell
pipenv --python=three
```

```shell
pip install scrapy redis pillow mysqlclient elasticsearch_dsl
```

```
import MySQLdb
```

ModuleNotFoundError: No module named 'MySQLdb'



# Python分布式爬虫打造搜索引擎

[Python分布式爬虫打造搜索引擎](http://coding.imooc.com/class/chapter/92.html)[聚学在线](http://www.projectsedu.com/ "Bobby老师的博客")
Windows(Python 3.5.3 Python 2.7.13)
Linux(Python 3.5.2+ Python 2.7.12+)
大神们写的笔记：
[今孝](http://www.cnblogs.com/jinxiao-pu/p/6706319.html)[天涯明月笙](http://www.jianshu.com/p/23b6d6b57ec5)[天涯明月笙-CSDN](http://blog.csdn.net/qq_23079443/article/details/73920584)

> 一盏灯， 一片昏黄； 一简书， 一杯淡茶。 守着那一份淡定， 品读属于自己的寂寞。 保持淡定， 才能欣赏到最美丽的风景！ 保持淡定， 人生从此不再寂寞。

## 第1章 课程介绍
介绍课程目标、通过课程能学习到的内容、和系统开发前需要具备的知识

### 可以使用爬虫的业务
- 互联网金融
- 数据建模
- 信息聚类
- 自然语言处理
- 医疗病例分析
- 数据分析服务

### 学习流程
1. 环境配置和基础知识铺垫
2. 爬取真实数据
3. scrapy突破反爬虫技术
4. scrapy进阶
5. scrapy redis 分布式爬虫
6. elasticsearch django实现搜索引擎

### 详细课程信息
#### 爬虫基础知识
1. 正则表达式
2. 深度优先和广度优先遍历算法
3. url去重的常见策略

#### Scrapy 爬取三个不同类型的网站
- 知名技术社区-[伯乐在线](http://www.jobbole.com)
- 知名问答网站-[知乎](https://www.zhihu.com)
- 知名招聘网站-[拉钩](https://www.lagou.com)

#### 网站结构和网络请求分析
#### xpath + css 提取数据
#### 模拟登陆
#### Scrapy模块
spider
item
item loader 
pipeline 
feed export 
CrawlSpider

#### 突破爬虫限制
- 图片验证码 网络打码平台
- ip访问频繁限制 代理池
- user-agent随机切换

#### Scrapy进阶开发
- scrapy的原理
- 基于scrapy的中间件开发
- 动态网站的抓取处理
- 将selenium和phantomjs集成到scrapy中
- scrapy log配置
- email发送
- scrapy信号

#### Scrapy-redis分布式爬虫
- 理解scrapy-redis分布式爬虫
- 集成bloomfilter到scrapy-redis中

#### Django和Elasticsearch搭建一套搜索引擎

### 课程体验-看完之后可以学会的技能
- 开发爬虫所需要用到的技术以及网站分析技巧
- 理解scrapy的原理和所有组件的使用以及分布式爬虫scrapy-redis的使用和原理
- 理解分布式开源搜索引擎elasticsearch的使用以及搜索引擎的原理
- 体验django如何快速搭建网站

## 第2章 Windows下搭建开发环境
介绍项目开发需要安装的开发软件、 python虚拟virtualenv和 virtualenvwrapper的安装和使用、 最后介绍pycharm和navicat的简单使用

### 开发用到的工具软件
1. IDE------PyCharm
2. 数据库------MySQL、Redis、Elasticsearch
3. 开发环境------pyenv、virtualenvwrapper

#### 课程中用到的版本
PyCharm 2016.3.2
Python 3.5.2+
MySQL 5.7.17
Navicat for MySQL

### 开发环境搭建
1. pycharm的安装和使用
2. mysql和navicat的安装和使用
3. virtualenv和virtualenvwrapper安装和配置
4. mysql 刷新权限命令，使root用户可以从所有IP远程连接。
```
GRANT ALL PRIVILEGES ON *.* TO 'root@'%' IDENTIFIED BY 'root' WITH GRANT OPTION; 
flush privileges;
```

#### 新建数据库
数据库名：scrapyspider
字符集：utf8 -- UTF-8 Unicode
排序规则：utf8_general_ci

#### 新建数据表
数据表名：article

|名|类型|长度|小数点|不是null|主键|
|:|
|addtime|datetime|0|0|||
|name|varchar|100|0|||
|id|int|10|0|yes|1|

#### macOS、Linux下Python安装-需要安装[pyenv](https://github.com/pyenv/pyenv)
##### Python 2.7.13
```bash
pyenv install 2.7.13
pyenv global 2.7.13
pyenv rehash
```

##### Python 3.5.3
```bash
pyenv install 3.5.3
pyenv global 3.5.3
pyenv rehash
```

#### virtualenvwrapper安装使用
##### virtualenvwrapper安装
```
pip install -i https://pypi.douban.com/simple/ virtualenvwrapper
```

##### virtualenvwrapper使用-需要配置环境变量
```
# 新建虚拟环境
mkvirtualenv test
# 新建虚拟环境-指定python版本
mkvirtualenv test --python=/path/python
# 使用虚拟环境
workon test
# 退出虚拟环境
deactivate
# 删除虚拟环境
rmvirtualenv test
```

NLPIR汉语分词系统


## 第3章 爬虫基础知识回顾
介绍爬虫开发中需要用到的基础知识包括爬虫能做什么，正则表达式，深度优先和广度优先的算法及实现、爬虫url去重的策略、彻底弄清楚unicode和utf8编码的区别和应用。

### 基础知识
#### 技术选型
1. requests和beautifulsoup都是库，scrapy是框架
2. scrapy框架中可以加入requests和beautifulsoup
3. scrapy基于twisted，性能是最大的优势
4. scrapy方便扩展，提供了很多内置的功能
5. scrapy内置的css和xpath selector非常方便，beautifualsoup最大的缺点就是慢

#### 网页分类
1. 静态网页
2. 动态网页
3. webservice（restapi）>属于动态页面的一种

#### 爬虫能做什么
1. 搜索引擎----百度、google、垂直领域搜索引擎
2. 推荐引擎----今日头条、一点资讯
3. 机器学习的数据样本
4. 数据分析(如金融数据分析)、舆情分析。

#### 正则表达式
##### 正则表达式介绍
1. 为什么必须学习正则表达式
2. 正则表达式的简单应用及python示例

##### 正则表达式基础
###### 1. 特殊字符
```
1) ^ $ * ? + {2} {2,} {2,5} |
2) [] [^] [a-z] .
3) \s \S \w \W
4) [\u4E00-\u9FA5] () \d
```

###### 特殊字符含义
```
^ 开头    '^b.*'----以b开头的任意字符

$ 结尾    '^b.*3$'----以b开头，3结尾的任意字符

* 任意长度（次数），≥0       

? 非贪婪模式，非贪婪模式尽可能少的匹配所搜索的字符串 '.*?(b.*?b).*'----从左至右第一个b和的二个b之间的内容（包含b）

+ 一次或多次

{2} 指定出现次数2次

{2,} 出现次数≥2次

{2,5} 出现次数2≤x≤5

| 或     例如，“z|food”能匹配“z”或“food”(此处请谨慎)。“[z|f]ood”则匹配“zood”或“food”。

[] 中括号中任意一个符合即可（中括号里面没有分转义字符）   '[abc]ooby123'----只要开头符合[]中任意一个即可

[^] 只要不出现[]中括号中的字符即可

[a-Z] 从小a到大Z        '1[48357][0-9]{9}'----电话号码

. 任意字符

\s 匹配不可见字符 \n \t    '你\s好'----可以匹配‘你 好’

\S 匹配可见字符，即普通字符

\w 匹配下划线在内的任何单词字符

\W 和上一个相反

[\u4E00-\u9FA5] 只能匹配汉字

() 要取出的信息就用括号括起来

\d 数字
```

###### 特殊字符实例
```python
import re

# "^"
line = "bobby123"
regex_str = "^a.*" # "^a"以a开头的字符串
if re.match(regex_str, line):
	print("yes")
	
# "?"
line = "boooooobby123"
# regex_str = ".*(b.*b).*" # 贪婪模式
regex_str = ".*?(b.*?b).*" # 非贪婪模式
match_obj = re.match(regex_str, line)
if match_obj:
	print("yes")

line = "XXX出生于2001年6月1日"
#line = "XXX出生于2001年6月"
#line = "XXX出生于2001/6/1"
#line = "XXX出生于2001-6-1"
#line = "XXX出生于2001-06-01"
#line = "XXX出生于2001-06"

regex_str = ".*出生于\d{4}[年/-]\d{1,2}([月/-]\d{1,2}日|[-/]\d{1,2}|$)"
match_obj = re.match(regex_str, line)
if match_obj:
    print(match_obj.group())
```



爬取策略的深度优先和广度优先

爬虫网址去重策略

基础环境

> Python 3.6.5  </br>
> PyCharm 2017.3.4 </br>
> MariaDB </br>
> Navicat </br>

`pip install scrapy`

命令行创建scrapy项目

`scrapy startproject ArticleSpider`

scrapy目录结构

> scrapy.cfg

配置文件

> setings.py
> 设置
> SPIDER_MODULES = ['ArticleSpider.spiders'] #存放spider的路径
> NEWSPIDER_MODULE = 'ArticleSpider.spiders'
> pipelines.py:

做跟数据存储相关的东西

> middilewares.py:

自己定义的middlewares 定义方法，处理响应的IO操作

> init.py:

项目的初始化文件。

> items.py：

定义我们所要爬取的信息的相关属性。Item对象是种类似于表单，用来保存获取到的数据

创建我们的spider

```
cd ArticleSpider
scrapy genspider jobbole blog.jobbole.com
```

scray在命令行启动Spyder的命令:

`scrapy crawl jobbole`

创建我们的调试工具类*

在项目根目录里创建main.py
作为调试工具文件

```py
import sys
import os

from scrapy.cmdline import execute


#将系统当前目录设置为项目根目录
sys.path.append(os.path.dirname(os.path.abspath(__file__)))
execute(["scrapy", "crawl" , "jobbole"])
```

开启控制台调试

`scrapy shell http://blog.jobbole.com/110287/`

twist异步机制

Scrapy使用了Twisted作为框架，Twisted有些特殊的地方是它是事件驱动的，并且比较适合异步的代码。在任何情况下，都不要写阻塞的代码。阻塞的代码包括:

- 访问文件、数据库或者Web
- 产生新的进程并需要处理新进程的输出，如运行shell命令
- 执行系统层次操作的代码，如等待系统队列



#### 深度优先和广度优先

1. 网站的树结构
2. 深度优先算法和实现----递归
3. 广度优先算法和实现----队列

##### 1. 网站的树结构
---
www.jobbole.com

---

top.jobbole.com blog.jobbole.com python.jobbole.com android.jobbole.com

---
top.jobbole.com/123
top.jobbole.com/456

blog.jobbole.com/123
blog.jobbole.com/456

---

##### 2. 深度优先算法和实现----递归
###### 深度优先
A
B C
D E  F G H
I

爬取顺序
A B D E I C F G H （递归实现）

###### 深度优先过程
```python
def depth_tree(tree_node):
	if tree_node is not None:
		print(tree_node._data)
		if tree_node._left is not None:
			return depth_tree(tree_node._left)
		if tree_node._right is not None:
			return depth_tree(tree_node._right)
```
递归的栈太深会导致溢出

##### 3. 广度优先算法和实现----队列
###### 广度优先

A
B C
D E  F G H
I

爬取顺序
A B C D E F G H I （队列实现）

###### 广度优先过程
```python
def level_queue(root):
	"""利用队列实现树的广度优先遍历"""
	if root is None:
		return
	my_queue = []
	node = root
	my_queue.append(node)
	while my_queue:
		node = my_queue:
		print(node.elem)
		if node.lchild is not None:
			my.queue.append(node.lchild)
		if node.rchild is not None:
			my.queue.append(node.rchild)
```

#### 爬虫去重策略
1. 将访问过的url保存到数据库中。 *(用的非常少，效率低，应用起来最简单的方式)*
2. 将访问过的url保存到set中，只要o(1)【这是常数阶时间复杂度】的代价就可以查询url 100000000x2bytex50个字符/1024/1024/1024≈9G 一亿条数据，一条50字符。 *(当数据量多时，占用内存非常高)*
3. url经过md5等方法哈希后保存到set中 *(比较常用)*
4. 用bitmap方法，将访问过的url通过hash函数映射到某一位 * (比md5方法更进一步压缩内存，冲突可能性非常大)*
5. bloomfilter方法对bitmap进行改进，多重hash函数降级冲突 *(减少内存，减少冲突。比url保存set减少的是量级的内存。1亿条大概几十MB的内存)*

#### 字符串编码
1. 计算机只能识别数字，文本转换为数字才能处理。计算机中8个bit作为一个字节，所以一个字节能表示最大的数字就是255.
2. 计算机是美国人发明的，所以一个字节可以表示所有字符了，所以ASCII（一个字节）编码就成为美国人标准编码。
3. 但是ASCII处理中文明显是不够的，中文不止255个汉字，所以中国制定了GB2312编码，用两个字节表示一个汉字。GB2312还把ASCII包含进去了，同理，日文，韩文等等上百个国家为了解决这个问题就都发展了一套字节的编码，标准就越来越多，如果出现多种语言混合就一定会出现乱码。
4. 于是unicode出现了，将所有语言统一到一套编码里。
5. ASCII和unicode编码：
 （1）字母A用ASCII编码十进制65，二进制01000001
 （2）汉字‘中’已超多ASCII编码的范围，用unicode编码是20013，二进制01001110 00101101
 （3）A用unicode编码中只需要前面补0，二进制是 00000000 01000001
6. 乱码问题解决可，但是如果内容全是英文，unicode编码比ASCII需要多一倍的存储空间，同时如果传输需要多一倍的传输。
7. 所以出现了可变长的编码“utf-8”，把英文变长一个字节，汉字3个字节。特别生僻的变成4-6字节。如果传输大量的英文，utf-8作用就很明显。

Unicode编码
读取：转换为unicode编码 保存：转换为utf-8编码
UTF-8编码 文件：bobby.txt 

```python
# python2
s = "abc"
su = u"abc"
s.encode("utf8")
su.encode("utf8")
s = "我用python"
su = u"我用python"
## windows
s.decode("gb2312").encode("utf-8")
su.encode("utf-8")
## linux
s.decode("utf8").encode("utf-8")
su.encode("utf-8")
# 获取系统默认的编码
sys.getdefaultencoding()

#python3
s="我用python"
s.encode("utf-8")
```

## 第4章 Scrapy爬取知名技术文章网站
搭建scrapy的开发环境，本章介绍scrapy的常用命令以及工程目录结构分析，本章中也会详细的讲解xpath和css选择器的使用。然后通过scrapy提供的spider完成所有文章的爬取。然后详细讲解item以及item loader方式完成具体字段的提取后使用scrapy提供的pipeline分别将数据保存到json文件以及mysql数据库中。...

### 新建开发环境
```
mkvirtualenv article_spider --python=/Users/jinlong/.pyenv/versions/3.6.3/bin/python
pip install scrapy
cd ~/PycharmProjects
scrapy startproject ArticleSpider
cd ArticleSpider
scrapy genspider jobbole blog.jobbole.com
```

### 编写爬虫代码
#### 调试爬虫的main.py
```
import sys
import os

from scrapy.cmdline import execute

sys.path.append(os.path.dirname(os.path.abspath(__file__)))
execute(["scrapy", "crawl", "jobbole"])
```

#### settings.py
```
ROBOTSTXT_
```

## 第5章 Scrapy爬取知名问答网站
本章主要完成网站的问题和回答的提取。本章除了分析出问答网站的网络请求以外还会分别通过requests和scrapy的FormRequest两种方式完成网站的模拟登录， 本章详细的分析了网站的网络请求并分别分析出了网站问题回答的api请求接口并将数据提取出来后保存到mysql中。...



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



## 第6章 通过CrawlSpider对招聘网站进行整站爬取
本章完成招聘网站职位的数据表结构设计，并通过link extractor和rule的形式并配置CrawlSpider完成招聘网站所有职位的爬取，本章也会从源码的角度来分析CrawlSpider让大家对CrawlSpider有深入的理解。



Windows下类似linux的终端 ·cmder·



数据库 article_spider

数据表 lagou_job

设计数据表结构

![](/img/lagou_job.png)



编写CrawlSpider

查看scrapy spider提供的模板

```shell
scrapy genspider --list
```

生成crawlspider

```shell
scrapy genspider -t crawl lagou www.lagou.com
```

变量PYTHONPATH

```python
import sys
import os


project_dir = os.path.abspath(os.path.dirname(__file__))
BASE_DIR = os.path.dirname(os.path.abspath(os.path.dirname(__file__)))
sys.path.insert(0, os.path.join(BASE_DIR, 'ArticleSpider'))
```

CrawlSpider是个什么类？

Rule



LinkExtractor



## 第7章 Scrapy突破反爬虫的限制
本章会从爬虫和反爬虫的斗争过程开始讲解，然后讲解scrapy的原理，然后通过随机切换user-agent和设置scrapy的ip代理的方式完成突破反爬虫的各种限制。本章也会详细介绍httpresponse和httprequest来详细的分析scrapy的功能，最后会通过云打码平台来完成在线验证码识别以及禁用cookie和访问频率来降低爬虫被屏蔽的可能性。...

## 第8章 Scrapy进阶开发
本章将讲解scrapy的更多高级特性，这些高级特性包括通过selenium和phantomjs实现动态网站数据的爬取以及将这二者集成到scrapy中、scrapy信号、自定义中间件、暂停和启动scrapy爬虫、scrapy的核心api、scrapy的telnet、scrapy的web service和scrapy的log配置和email发送等。 这些特性使得我们不仅只是可以通过scrapy来完成...

## 第9章 Scrapy-redis分布式爬虫
Scrapy-redis分布式爬虫的使用以及scrapy-redis的分布式爬虫的源码分析， 让大家可以根据自己的需求来修改源码以满足自己的需求。最后也会讲解如何将bloomfilter集成到scrapy-redis中。

## 第10章 Elasticsearch搜索引擎的使用
本章将讲解elasticsearch的安装和使用，将讲解elasticsearch的基本概念的介绍以及api的使用。本章也会讲解搜索引擎的原理并讲解elasticsearch-dsl的使用，最后讲解如何通过scrapy的pipeline将数据保存到elasticsearch中。



[Open Source Search & Analytics · Elasticsearch](https://www.elastic.co/)

[Elastic 中文社区](https://elasticsearch.cn/)

[Apache Solr](http://lucene.apache.org/solr/)

[Sphinx | Open Source Search Engine](http://sphinxsearch.com/)

[Apache Lucene - Welcome to Apache Lucene](http://lucene.apache.org/)



[ELK Stack: Elasticsearch, Logstash, Kibana | Elastic](https://www.elastic.co/elk-stack)



# 简介

### **关系数据搜索缺点**

- MySQL

1. 无法打分
2. 无分布式
3. 无法解析搜索请求
4. 效率底（上亿数据量）
5. 分词



### **NoSQL 数据库（Not only SQL）**

- MongoDB
- Redis
- Elasticsearch 主要用途**全文搜索引擎**



# 安装 Elasticsearch

1. 安装 elasticsearch-rtf 
2. head 插件和 kibana 安装



安装 jdk8 以上，本文使用 jdk8u121。

java version 1.8.0_111



## 安装 elasticsearch-rtf 

**安装 elasticsearch-rtf**

```shell
git clone git://github.com/medcl/elasticsearch-rtf.git -b master --depth 1
```



**配置文件**

 elasticsearch-rtf/config/elasticsearch.yml



 elasticsearch-rtf/data 数据文件

 elasticsearch-rtf/lib 库

 elasticsearch-rtf/modules 模块

 elasticsearch-rtf/plugins 插件



**运行**

```shell
elasticsearch-rtf/bin/elasticsearch
```

[libac/elasticsearch-rtf - Docker Hub](https://hub.docker.com/r/libac/elasticsearch-rtf)

```shell
docker run -d --name elasticsearch-rtf -p 9200:9200 -p 9300:9300 libac/elasticsearch-rtf
```



**访问**

http://localhost:9200



## 安装 head 插件和 kibana

- head 插件：基于浏览器的插件可以完成类似于 Navicat 的功能
- 使用的是 kibana 项目中的一个 sense。



### elasticsearch-head

[mobz/elasticsearch-head: A web front end for an elastic search cluster](https://github.com/mobz/elasticsearch-head)

```shell
git clone git://github.com/mobz/elasticsearch-head.git
cd elasticsearch-head
npm install
npm run start
```

[mobz/elasticsearch-head - Docker Hub](https://hub.docker.com/r/mobz/elasticsearch-head/)

```shell
docker run -d --name elasticsearch-head-5 -p 9100:9100 mobz/elasticsearch-head:5
```



**访问**

http://localhost:9100/



**head 访问 elasticsearch 设置**

 elasticsearch.yml

```yaml
http.cors.enabled: true
http.cors.allow-origin: "*"
http.cors.allow-methods: OPTIONS, HEAD, GET, PUT, DELETE
http.cors.allow-headers: "X-Requested-With, Content-Type, Content-Length, X-User"
```



### kibana sense

kibana 版本需要和 elasticsearch 版本一致。

[Kibana 5.1.1 | Elastic](https://www.elastic.co/downloads/past-releases/kibana-5-1-1)

讲课用的是 windows 版本 kibana 5.1.2



[Running Kibana on Docker](https://www.elastic.co/guide/en/kibana/current/docker.html)

[kibana - Docker Hub](https://hub.docker.com/_/kibana)

```shell
docker pull kibana:5.1.1
```



**运行**

```shell
docker run -d --name kibana-5.1.1 -p 5601:5601 -e ELASTICSEARCH_URL=http://172.19.241.212:9200 kibana:5.1.1

docker stop kibana-5.1.1
docker rm kibana-5.1.1

# 查看容器 ip 地址
docker inspect --format '{{ .NetworkSettings.IPAddress }}' kibana-5.1.1

# elasticsearch.yml
network.host: 0.0.0.0
```



**访问**

http://localhost:5601



[Console - Kibana](http://localhost:5601/app/kibana#/dev_tools/console?_g=())

Sense 改名为 Dev Tools。



# elasticsearch 概念

1. **集群**：一个或者多个节点组织在一起。
2. **节点**：一个节点是集群中的一个服务器，由一个名字来标识，默认是一个随机的漫画角色的名字
3. **分片**：将索引划分为多份的能力，允许水平分割和扩展容量，多个分片响应请求，提高性能和吞吐量
4. **副本**：创建分片的一份或多份的能力，在一个节点失败其余节点可以顶上



**mysql 和 elasticsearch 概念对比**


| elasticsearch   | mysql  |
| :-------------- | ------ |
| index(索引)     | 数据库 |
| type(类型)      | 表     |
| documents(文档) | 行     |
| fields          | 列     |



# HTTP方法

- HTTP1.0 定义三种请求方式：GET, POST 和 HEAD 方法
- HTTP1.1 新增了五种方法：OPTIONS, PUT, DELETE, TRACE, CONNECT

| 方法   | 描述                                     |
| :----- | ---------------------------------------- |
| GET    | 请求指定的页面信息，并返回实体主体       |
| POST   | 向服务器提交数据进行处理。               |
| PUT    | 向服务器传送的数据取代指定的文档的内容。 |
| DELETE | 请求服务器删除指定的页面。               |



# 倒排索引

由属性值来确定记录的位置。



**Python 写各大聊天系统的屏蔽脏话功能原理**

| 关键词   | 文章         |
| :------- | ------------ |
| python   | 文章1，文章3 |
| 聊天     | 文章2        |
| 系统     | 文章3，文章4 |
| 屏蔽脏话 | 文章5        |



文章中**关键词**频率高，权重就高。



| 关键词   | 倒排列表                    |
| :------- | --------------------------- |
| python   | （文章1，<2，10>，2）       |
| 聊天     | （文章2，<12，25，100>，3） |
| 系统     | （文章3，\<10 >，1）        |
| 屏蔽脏话 | （文章5，<50，60>，2）      |

> **TF-IDF**：elasticsearch 打分



## 倒排索引待解决的问题

1. 大小写转换问题，如 python 和 PYTHON 应该为一个词
2. 词干抽取，looking 和 look 应该处理为一个词
3. 分词，如**屏蔽系统**应该分词为”屏蔽“、”系统“还是应该分为”屏蔽系统“
4. 倒排索引文件过大 - 压缩编码



# elasticsearch 的文档和索引的 CRUD 操作

> CRUD：增删改查

``` yaml
# es 的文档、索引的 CRUD 操作

# 索引初始化操作
# 指定分片和副本的数量
# shards 一旦设置不能修改

PUT lagou
{
  "settings": {
    "index": {
      "number_of_shards":5,
      "number_of_replicas":1
    }
  }
}
```

elasticsearch-head 索引中可以新建和删除 index。



获取信息

```yaml
# 获取 _settings 信息
GET lagou/_settings
GET _all/_settings
GET .kibana,lagou/_settings
GET _settings
```



修改信息

```yaml
# 修改 _settings 信息
PUT lagou/_settings
{
  "number_of_replicas": 3
}
```



获取索引信息

```yaml
# 获取索引信息
GET _all
GET lagou
```



保存文档

```yaml
# 保存文档
# lagou 是 index
# job 是 type
# 1 是文档 id _id
PUT lagou/job/1
{
  "title":"python分布式爬虫开发",
  "salary_min":15000,
  "city":"北京",
  "company":{
    "name":"百度",
    "company_addr":"北京市软件园"
  },
  "publish_date":"2017-4-16",
  "comments":15
}

# 自动生成 _id
POST lagou/job/
{
  "title":"python django 开发工程师",
  "salary_min":30000,
  "city":"上海",
  "company":{
    "name":"美团科技",
    "company_addr":"北京市软件园A区"
  },
  "publish_date":"2017-4-16",
  "comments":20
}
```



获取文档

```yaml
#
GET lagou/job/1
#
GET lagou/job/1?_source=title
#
GET lagou/job/1?_source=title,city
#
GET lagou/job/1?_source
```



修改文档

```yaml
# 覆盖修改
PUT lagou/job/1
{
  "title":"python分布式爬虫开发",
  "salary_min":15000,
  "company":{
    "name":"百度",
    "company_addr":"北京市软件园"
  },
  "publish_date":"2017-4-16",
  "comments":15
}

# 增量修改，修改某一个字段
POST lagou/job/1/_update
{
  "doc": {
    "comments":20
  }
}
```



删除

```yaml
# 删除文档
DELETE lagou/job/1
# 删除索引 index
DELETE lagou
```



# elasticsearch 批量操作

> mget 批量获取和 bulk 批量操作

## mget

```yaml
# mget 一次查询多条记录
GET _mget
{
  "doc":[
    {"_index":"testdb1",
      "_type":"job1",
      "_id":1
    },
    {"_index":"testdb2",
      "_type":"job2",
      "_id":2
    }
    ]
}

# 同一个 index 下 mget
GET testdb/_mget
{
  "doc":[
    {"_type":"job1",
      "_id":1
    },
    {_type":"job2",
      "_id":2
    }
    ]
}

# 同一个 type 下 mget
GET testdb/job1/_mget
{
  "doc":[
    {"_id":1
    },
    {"_id":2
    }
    ]
}

# 同一个 type 下 mget，简写方式
GET testdb/job1/_mget
{
  "ids":[1,2]
}
```



## bulk 批量操作

批量导入可以合并多个操作，比如 index，delete，update，create 等等。也可以帮助从一个索引导入到另一个索引。

```json
POST _bulk
{"index":{"_index":"lagou","_type":"job1","_id":"1"}}
{"title":"python分布式爬虫开发","salary_min":15000,"city":"北京","company":{"name":"百度","company_addr":"北京市软件园"},"publish_date":"2017-4-16","comments":15}
{"index":{"_index":"lagou","_type":"job2","_id":"2"}}
{"title":"python django开发","salary_min":30000,"city":"成都","company":{"name":"阿里巴巴","company_addr":"北京市软件园B区"},"publish_date":"2017-4-18","comments":50}
```

```json
{ "index" : { "_index" : "test", "_type" : "type1", "_id" : "1" } }
{ "field1" : "value1" }

{ "delete" : { "_index" : "test", "_type" : "type1", "_id" : "2" } }

{ "create" : { "_index" : "test", "_type" : "type1", "_id" : "3" } }
{ "field1" : "value3" }

{ "update" : {"_id" : "1", "_type" : "type1", "_index" : "index1"} }
{ "doc" {"field2" : "value2"} }
```

bulk 发送数据到一个节点去解析，然后分发给其他节点分片进行操作。



# elasticsearch 的 mapping 映射管理

映射(mapping) [mapping-store | Elasticsearch Reference \[6.5] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-store.html)

**映射**：创建索引的时候，可以预先定义字段的类型以及相关属性

> 每个字段是什么类型的以及有什么属性。

elasticsearch 会根据 JSON 源数据的基础类型猜测你想要的字段映射，将输入的数据转变成可搜索的索引项。Mapping 就是我们自己定义字段的数据类型，同时告诉 elasticsearch 如何索引数据以及是否可以被搜索。

**作用**：会让索引建立的更加细致和完善

**类型**：**静态映射**和**动态映射**

## 内置类型

- string 类型：text，keyword（string 类型在 es5 开始已经废弃）
- 数字类型：long，integer，short，byte，double，float
- 日期类型：date（datetime 也可以解析）
- bool 类型：boolean
- binary 类型：binary（二进制数据类型不会被检索）
- 复杂类型：object，nested
- geo 类型：geo-point，geo-shape
- 专业类型：ip，competion



object 类型：

```json
"company":{"name":"百度","company_addr":"北京市软件园"}
```

> company 就是 object 类型。



nested 类型：

```json
"company":{
    "name":"百度",
    "company_addr":"北京市软件园",
    "emplyee":[{
    {
    "name":"百度",
    "age":28
    },
    {
    "name":"百度",
    "age":28
    }}]
}
```

> object 类型放到数组里面，就是 nested 类型。
>
> emplyee 就是 nested 类型。



## 属性

| 属性           | 描述                                                         | 适合类型 |
| :------------- | ------------------------------------------------------------ | -------- |
| store          | 值为 yes 表示存储，为 no 表示不存储，默认为 no               | all      |
| index          | yes 表示分析，no 表示不分析，默认为 true                     | string   |
| null_value     | 如果字段为空，可以设置一个默认值，比如“NA”                   | all      |
| analyzer       | 可以设置索引和搜索使用的分析器，默认使用的是 standard 分析器，还可以使用 whitespace、simple、english | all      |
| include_in_all | 默认 es 为每个文档定义一个特殊域 _all，它的作用是让每个字段被搜索到，如果不想某个字段被搜索到，可以设置为 false | all      |
| format         | 时间格式字符串的模式                                         | date     |



## mapping

新建 mapping

```yaml
# 新建 mapping
PUT lagou
{
  "mappings": {
    "job":{
      "properties": {
        "title":{
          "type": "text"
        },
        "salary_min":{
          "type": "integer"
        },
        "city":{
          "type": "keyword"
        },
        "company":{
          "properties": {
            "name":{
              "type":"text"
            },
            "company_addr":{
              "type":"text"
            },
            "employee_count":{
              "type":"integer"
            }
          }
        },
        "publish_date":{
          "type": "date",
          "format": "yyyy-mm-dd"
        },
        "comments":{
          "type": "integer"
        }
      }
    }
  }
}
```

获取 mapping 信息

```yaml
GET lagou/_mapping/job
GET _all/_mapping/job
```

> mapping 一旦创建了类型就不能修改。



# elasticsearch 查询

elasticsearch 是功能非常强大的搜索引擎，使用它的目的是为了快速的查询到需要的数据。



## 查询分类

- 基本查询：使用 elasticsearch 内置查询条件进行查询
- 组合查询：把多个查询组合在一起进行复合查询
- 过滤：查询同时，通过 filter 条件在不影响打分的情况下筛选数据



## 基本查询

**测试数据准备**

```yaml
# mapping
PUT lagou
{
  "mappings": {
    "job":{
      "properties": {
        "title":{
          "store": true,
          "type": "text",
          "analyzer": "ik_max_word"
        },
        "company_name":{
          "store": true,
          "type": "keyword"
        },
        "desc":{
          "type": "text"
        },
        "comments":{
          "type": "integer"
        },
        "add_time":{
          "type": "date",
          "format": "yyyy-mm-dd"
        }
      }
    }
  }
}

DELETE lagou

# 添加数据 1
POST lagou/job/
{
  "title":"python django 开发工程师",
  "company_name":"美团科技有限公司",
  "desc":"对django的概念熟悉，熟悉python基础知识",
  "comments":20,
  "add_time":"2017-4-1"
}

# 添加数据 2
POST lagou/job/
{
  "title":"python scrapy redis分布式爬虫基本",
  "company_name":"百度科技有限公司",
  "desc":"对scrapy的概念熟悉，熟悉redis的基本操作",
  "comments":5,
  "add_time":"2017-4-15"
}

# 添加数据 3
POST lagou/job/
{
  "title":"elasticsearch打造搜索引擎",
  "company_name":"阿里巴巴科技有限公司",
  "desc":"熟悉数据结构算法，熟悉python的基本开发",
  "comments":15,
  "add_time":"2017-6-20"
}

# 添加数据 4
POST lagou/job/
{
  "title":"python打造推荐引擎系统",
  "company_name":"阿里巴巴科技有限公司",
  "desc":"熟悉推荐引擎的原理以及算法，掌握C语言",
  "comments":60,
  "add_time":"2016-10-20"
}
```



match 查询

```yaml
# 查询 python 关键词
GET lagou/job/_search
{
  "query": {
    "match": {
      "title": "python"
    }
  }
}
```



term 查询

```yaml
# term 查询，只能查询到”阿里巴巴科技有限公司“
GET lagou/_search
{
  "query": {
    "term": {
        "company_name": "阿里巴巴科技有限公司"
    }
  }
}
```

> term 查询，不会对查询的词做处理



terms 查询

```yaml
# terms 查询，查到"工程师", "django", "系统"任意一个。
GET lagou/_search
{
  "query": {
    "terms": {
      "title": ["工程师", "django", "系统"]
    }
  }
}
```



控制查询的返回数量

```yaml
# from 从哪个开始，size 返回的条数
GET lagou/_search
{
  "query": {
    "match": {
      "title": "python"
    }
  },
  "from": 0,
  "size": 3
}
```



match_all 查询

```yaml
GET lagou/_search
{
  "query": {
    "match_all": {}
  }
}
```

> 返回所有数据



match_phrase 查询（短语查询 ）

```yaml
# match_phrase 必须满足”python系统”这个词
# slop 词之间的距离，“python“和”系统”之间可以有6个词的距离。例如匹配："python打造推荐引擎系统"
GET /lagou/_search
{
  "query": {
    "match_phrase": {
      "title": {
        "query": "python系统",
        "slop": 6
      }
    }
  }
}
```



multi_match 查询

```yaml
# title^3， titile 的权重是 desc 的 3倍。
GET lagou/_search
{
  "query": {
    "multi_match": {
      "query": "python",
      "fields": ["title^3", "desc"]
    }
  }
}
```

> 可以指定多个字段，比如查询 title 和 desc 这两个字段里面包含 python 的关键词文档



指定返回字段

```yaml
# stored_fields，mapping 设置中 store 为 ture 的字段返回
GET lagou/_search
{
  "stored_fields": ["title","company_name"],
  "query": {
    "match": {
      "title": "python"
    }
  }
}
```



通过 sort 把结果排序

```yaml
# comments 字段，desc 降序排序
GET lagou/_search
{
  "query": {
    "match_all": {}
  },
  "sort": [
    {
      "comments": {
        "order": "desc"
      }
    }
  ]
}
```



查询范围

```yaml
# comments 字段
# gte 大于等于，lte 小于等于，boost 权重
GET lagou/_search
{
  "query": {
    "range": {
      "comments": {
        "gte": 10,
        "lte": 20,
        "boost": 2.0
      }
    }
  }
}
```



时间范围查询

```yaml
GET lagou/_search
{
  "query": {
    "range": {
      "add_time": {
        "gte": "2017-04-01",
        "lte": "now"
      }
    }
  }
}
```



wildcard 查询（简单的模糊查询）

```yaml
# pyth*n，模糊匹配
GET lagou/_search
{
  "query": {
    "wildcard": {
      "title": {
        "value": "pyth*n",
        "boost": 2.0
      }
    }
  }
}
```



## 组合查询

**建立测试数据**

```yaml
POST lagou/testjob/_bulk
{"index":{"_id":1}}
{"salary":10, "title":"Python"}
{"index":{"_id":2}}
{"salary":20, "title":"Scrapy"}
{"index":{"_id":3}}
{"salary":30, "title":"Elasticsearch"}
{"index":{"_id":4}}
{"salary":40, "title":"Elasticsearch"}
```



### bool 查询

> 老版本的 filtered 已经被 bool 替换
>
> 用 bool 包括 must should must_not  filter 来完成

- filter：字段过滤，不参与打分的
- must： 所有查询都必须同时满足
- should：查询条件满足一个或多个
- must_not：查询条件一个都不用满足



简单过滤查询

> SQL 语句：`select * from testjob where salary=20`
>
> 薪资为20k的工作

```yaml
# 简单过滤查询
GET lagou/testjob/_search
{
  "query": {
    "bool": {
      "must": {
        "match_all": {}
      }, 
      "filter": {
        "term": {
          "salary": 20
        }
      }
    }
  }
}

# must 可以省略
GET lagou/testjob/_search
{
  "query": {
    "bool": {
      "filter": {
        "term": {
          "salary": 20
        }
      }
    }
  }
}
```



查询多个指定值

```yaml
GET lagou/testjob/_search
{
  "query": {
    "bool": {
      "filter": {
        "terms": {
          "salary":[10,20]
        }
      }
    }
  }
}
```



查询指定值

> SQL 语句：`select * from testjob where title="Python"`

```yaml
GET lagou/testjob/_search
{
  "query": {
    "bool": {
      "filter": {
        "terms": {
          "title": "python"
        }
      }
    }
  }
}
```



**查看 ik 分析器解析的结果**

分词器 ik [medcl/elasticsearch-analysis-ik: The IK Analysis plugin integrates Lucene IK analyzer into elasticsearch, support customized dictionary.](https://github.com/medcl/elasticsearch-analysis-ik)

```yaml
# 默认分词器
GET _analyze
{
  "text": "Python网络开发工程师"
}

# ik_max_word
GET _analyze
{
  "analyzer": "ik_max_word",
  "text": "Python网络开发工程师"
}

# ik_smart，最小的数据集合分割词组
GET _analyze
{
  "analyzer": "ik_smart",
  "text": "Python网络开发工程师"
}
```



bool 过滤查询，可以做组合过滤查询

> SQL 语句：`select * from testjob where (salary=20 OR title=Python) AND (salary != 30)`
>
> 查询薪资等于20k或者工作为 python 的工作，排除价格为30k的

```yaml
GET lagou/testjob/_search
{
  "query": {
    "bool": {
      "should": [
        {"term": {"salary": 20}},
        {"term": {"title": "python"}}
      ],
      "must_not": [
        {"term": {"salary": 30}},
        {"term": {"salary": 10}}
      ]
    }
  }
}
```



嵌套查询

> SQL 语句：`select * from testjob where title="python" or (title="elasticsearch" AND salary=30)`

```yaml
GET lagou/testjob/_search
{
  "query": {
    "bool": {
      "should": [
        {"term":{"title":"python"}},
        {"bool":{
          "must": [
            {"term":{"title":"elasticsearch"}},
            {"term":{"salary":30}}
          ]
         }
        }
      ]
    }
  }
}
```



### 过滤空和非空数据

**测试数据准备**

```yaml
POST lagou/testjob2/_bulk
{"index":{"_id":1}}
{"tags":["search"]}
{"index":{"_id":2}}
{"tags":["search", "python"]}
{"index":{"_id":3}}
{"other_field":["some data"]}
{"index":{"_id":4}}
{"tags":null}
{"index":{"_id":5}}
{"tags":["search", null]}
```



处理 null 空值的方法

> SQL 语句：`select tags from testjob2 where tags is not NULL`

```yaml
# tags 值不为空，参数存在的
GET lagou/testjob2/_search
{
  "query": {
    "bool": {
      "filter": {
        "exists": {
          "field": "tags"
        }
      }
    }
  }
}

# tags 值为空，参数不存在的
GET lagou/testjob2/_search
{
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "tags"
        }
      }
    }
  }
}
```


# 搜索建议接口

[Completion Suggester | Elasticsearch Reference [6.5] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-suggesters-completion.html)

```yaml
PUT music
{
  "mappings": {
    "song": {
      "properties": {
        "suggest": {
          "type": "completion"
        },
        "title": {
          "type": "keyword"
        }
      }
    }
  }
}
```



# Fuzzy Query（模糊搜索）

```yaml
GET jobbole/_search
{
  "query": {
    "fuzzy": {"title" : "linx"}
  },
  "_source":["title"]
}
```



> 编辑距离：编辑距离是一种字符串之间相似程度的计算方式。
>
> 例如： linux 和 linx，添加 u，两个字符串相同，遍历距离为1。

```yaml
# fuzziness 编辑距离
# prefix_length 不能编辑的距离，lin 不能动，然后 un 和 x 对比编辑距离。
GET /_search
{
  "query": {
    "fuzzy": {
      "title": {
        "value": "linux",
        "fuzziness": 2,
        "prefix_length": 0
      }
    }
  },
  "_source": "title"
}
```



```yaml
POST jobbole/_search?pretty
{
  "suggest": {
    "my-suggest": {
      "text": "linux",
      "completion": {
        "field": "suggest",
        "fuzzy": {
          "fuzziness": 2
        }
      }
    }
  },
  "_source": "title"
}
```



## Highlighting 关键字高亮显示

```yaml
"highlight": {
	"pre_tags": ['<span class="keyWord">'],
	"post_tags": ['</span>'],
    "fields": {
        "title": {},
        "content": {}
    }
}
```

```yaml
POST jobbole/_search
{
  "query": {
    "multi_match": {
      "query": "linux",
      "fields": ["tags", "title", "content"]
    }
  },
  "from": 0,
  "size": 10,
  "highlight": {
    "pre_tags": ["<span class='keyWord'>"],
    "post_tags": ["</span>"],
    "fields": {
      "title": {},
      "content": {}
    }
  }
}
```



# scrapy 写入数据到 elasticsearch 中

编写 pipelines.py

```python
from models.es_types import ArticleType
from w3lib.html import remove_tags


class ElasticsearchPipeline(object):
    """将数据写入到 es 中"""
    
    def process_item(self, item, spider):
        # 将 item 转化为 es 的数据
        item.save_to_es()
        
        return item
```



## elasticsearch-dsl

[elastic/elasticsearch-dsl-py: High level Python client for Elasticsearch](https://github.com/elastic/elasticsearch-dsl-py)

```shell
pip install elasticsearch-dsl
```

[Persistence — Elasticsearch DSL 6.3.1 documentation](https://elasticsearch-dsl.readthedocs.io/en/latest/persistence.html)



### 生成 mapping

新建 models 包，models 包和 pipelines.py 在同一级目录。

新建 models/es_types.py 文件

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

from datetime import datetime
from elasticsearch_dsl import DocType, Data, Nested, Boolean, analyzer, InnerObjectWrapper, Completion Keyword, Text
from elasticsearch_dsl.analysis import CustomAnalysis as _CustomAnalysis
from elasticsearch_dsl.connections import connections


connections.create_connection(host=["localhost"])


class CustomAnalyzer(_CustomAnalysis):
    def get_analysis_definition(self):
        return {}
    

ik_analyzer = CustomAnalyzer("ik_max_word", filter=["lowercase"])


class ArticleType(DocType):
    # 伯乐在线文章类型
    suggest = Completion(analyzer=ik_analyzer)
    title = Text(analyzer="ik_max_word")
    create_date = Date()
    url = Keyword()
    url_object_id = Keyword()
    front_image_url = Keyword()
    front_image_path = Keyword()
    praise_nums = Integer()
    comment_nums = Integer()
    fav_nums = Integer()
    tags = Text(analyzer="ik_max_word")
    content = Text(analyzer="ik_max_word")
    
    class Meta:
        index = "jobbole"
        doc_type = "article"
        
        
if __name__ == "__main__":
    # 生成 mapping
    ArticleType.init()
```



items.py

```python
from models.es_types import ArticleType
from elasticsearch_dsl.connections import connections


es = connections.create_connection(ArticleType._doc_type.using)


def gen_suggest(index, info_tuple):
    # 根据字符串生成搜索建议数组
    used_words = set()  # 去重
    suggests = []
    for text, weight in info_tuple:
        if text:
            # 调用 es 的 analyze 接口分析字符串，进行分词。
            words = es.indices.analyze(index=index, analyzer="ik_max_word", params={'filter':["lowercase"]}, body=text)
            analyzed_words = set([r["token"] for r in words if len(r["token"])>1])
            new_words = analyzed_words - used_words
        else:
            new_words = set()
            
        if new_words:
            suggests.append({"input":list(new_words), "weight":weight})
    
    return suggests


class JobBoleArticleItem(scrapy.Item):
    
    def save_to_es(self):
        article = ArticleType()
        article.title = self['title']
        article.create_date = self['create_date']
        article.content = remove_tags(self['content'])
        article.front_image_url = self['front_image_url']
        if "front_image_path" in self:
            article.front_image_path = self['front_image_path']
        article.praise_nums = self['praise_nums']
        article.fav_nums = self['fav_nums']
        article.comments_nums = self['comments_nums']
        article.url = self['url']
        article.tags = self['tags']
        article.meta.id = self['url_object_id']
        
        article.suggest = gen_suggests(ArticleType._doc_type.index, ((article.title, 10),(article.tags, 7)))
        
        article.save()
        
        return
```


## 第11章 Django搭建搜索网站

本章讲解如何通过django快速搭建搜索网站， 本章也会讲解如何完成django与elasticsearch的搜索查询交互。

# 通过 django 和 elasticsearch 搭建搜索网站

开始写代码前，要了解系统的功能。



# django



## 新建虚拟环境

```shell
mkvirtualenv --python=3.5.0 lcv_search
```

> 这里我要提醒大家的是，大家最好跟我课程里的 Python 版本保持一致，不然后边的话有可能会出现问题。大家在学习的时候尽跟我的版本保持一致，大家在开发完成之后，自己再去尝试自己的新版本。

> 我使用的是 Python 3.6.1



**安装 django**

```shell
pip install django==1.11
```

```shell
pipenv install --pypi-mirror=https://pypi.douban.com/simple django==1.11
```



**安装 elasticsearch-dsl**

```shell
pip install elasticsearch-dsl==5.2.0
```

```shell
pipenv install --pypi-mirror=https://pypi.douban.com/simple elasticsearch-dsl==5.2.0
```



**安装 redis**

```shell
pip install redis==2.10.5
```

```shell
pipenv install --pypi-mirror=https://pypi.douban.com/simple redis==2.10.5
```



## 新建项目

PyCharm 中 新建项目 django 项目 `LcvSearch`

- Appcation Name：`search`
- Enable Django admin



```shell
# 新建项目
django-admin startproject LcvSearch

# 新建 App
python manage.py startapp search

# 运行
python manage.py runserver 127.0.0.1:8000
```



**添加 app** 到 settings.py

LcvSearch/settings.py

```python
INSTALLED_APPS = [
    'django.contrib.staticfiles',
    'search',
]
```



**配置静态页面**

新建目录 static。（和manage.py 同一目录，此目录可以随意命名，可配置。）（放置 css, js 文件）

新建目录 templates。（index.html, result.html 放在这个目录）（放置 html 模板文件）



**配置 url**

LcvSearch/LcvSearch/urls.py

```python
from django.views.generic import TemplateView

url(r'^$', TemplateView.as_view(template_name="index.html"), name="index"),
```



**配置  templates 路径**

settings.py

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR, "templates")],
]
```



**配置 static 路径** 

settings.py

```python
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "static")
]

# 也可以用 tuple，后面要加逗号。
STATICFILES_DIRS = ()
    os.path.join(BASE_DIR, "static"),
)
```



**修改 html 文件，添加路径。**

index.html

```html
{% load staticfiles %}

{% static 'css/style.css' %}
{% static 'js/global.js' %}
```



## suggest 搜索建议（自动补全）

**配置 url**

LcvSearch/urls.py

```python
from search.views import SearchSuggest


url(r'^suggest/$', SearchSuggest.as_view(), name="suggest"),
```



**Model 逻辑**

search/models.py

```python
from datetime import datetime
from elasticsearch_dsl import DocType, Data, Nested, Boolean, analyzer, InnerObjectWrapper, Completion Keyword, Text
from elasticsearch_dsl.analysis import CustomAnalysis as _CustomAnalysis
from elasticsearch_dsl.connections import connections


connections.create_connection(host=["localhost"])


class CustomAnalyzer(_CustomAnalysis):
    def get_analysis_definition(self):
        return {}
    

ik_analyzer = CustomAnalyzer("ik_max_word", filter=["lowercase"])


class ArticleType(DocType):
    # 伯乐在线文章类型
    suggest = Completion(analyzer=ik_analyzer)
    title = Text(analyzer="ik_max_word")
    create_date = Date()
    url = Keyword()
    url_object_id = Keyword()
    front_image_url = Keyword()
    front_image_path = Keyword()
    praise_nums = Integer()
    comment_nums = Integer()
    fav_nums = Integer()
    tags = Text(analyzer="ik_max_word")
    content = Text(analyzer="ik_max_word")
    
    class Meta:
        index = "jobbole"
        doc_type = "article"
        
        
if __name__ == "__main__":
    # 生成 mapping
    ArticleType.init()
```



**View 逻辑**

search/views.py

```python
import json

from django.views.generic.base import View
from django.http import HttpResponse
from search.models import ArticleType
# def suggest(request):


class SearchSuggest(View):
    
    def get(self, request):
        key_words = request.GET.get('s', '')
        re_datas = []
        if key_words:
            s = ArticleType.search()
            s = s.suggest('my_suggest', key_words, completion={
                "field": "suggest",
        		"fuzzy": {
          			"fuzziness": 2
        		},
                "size": 10
            })
            suggestions = s.execute_suggest()
            # for match in getattr(suggestions, "my_suggest")[0].options:
            for match in suggestions.my_suggest[0].options:
                source = match._source
                re_datas.append(source["title"])
         return HttpResponse(json.dumps(re_datas), content_type="application/json")
```



调试代码

```python
s = ArticleType.search()

# 此处打断点 s = s.suggest('my-suggest', keywords, completion={
s = s.suggest('my-suggest', keywords, completion={
    "field": "suggest",
    "fuzzy": {
        "fuzziness": 2
    },
    "size": 10
})
suggestions = s.execute_suggest()
for match in suggestions:
    pass
```



**html 配置**

index.html

```html
var suggest_url = "{% url "suggest" %}"
```



## 搜索功能

**配置 url**

LcvSearch/urls.py

```python
url(r'^search/$', SearchView.as_view(), name="search"),
```



**配置 View**

search/views.py

```python
from datetime import datetime

from elasticsearch import Elasticsearch


client = Elasticsearch(hosts=["127.0.0.1"])


class SearchView(View):
    
    def get(self, request):
        key_words = request.GET.get("q", "")
        page = request.GET.get("p", 1)
        try:
            page = int(page)
        except:
            page = 1
        start_time = datetime.now()
        response = client.search(
            index="jobbole",
            body={
                "query":{
                    "multi_match":{
                        "query": key_words,
                        "fields": ["tags", "title", "content"]
                    }
                },
                "form":0,
                "size":10,
                "highlight": {
                    "pre_tags": ['<span class="keyWord">'],
                    "post_tags": ['</span>'],
                    "fields": {
                        "title": {},
                        "content": {}
                    }
                }
            }
        )
        
        end_time = datetime.now()
        last_seconds = (end_time-start_time).total_seconds()
        total_nums = response["hits"]["total"]
        if (page%10) > 0:
            page_nums = int(total_nums/10) + 1
        else:
            page_nums = int(total_nums/10)
        hit_list = []
        for hit in response["hits"]["hits"]:
            hit_dict = {}
            if "title" in hit["highlight"]:
                hit_dict["title"] = "".join(hit["highligh"]["title"])
            else:
                hit_dict["title"] = hit["_source"]["title"]
            if "content" in hit["highlight"]:
                hit_dict["content"] = "".join(hit["highligh"]["content"])[:500]
            else:
                hit_dict["content"] = hit["_source"]["content"][:500]
                
            hit_dict["create_date"] = hit["_source"]["create_date"]
            hit_dict["url"] = hit["_source"]["url"]
            hit_dict["score"] = hit["_score"]
            
            hit_list.append(hit_dict)
            
        return render(request, "result.html", {"page":page, "all_hits": hit_list, "key_words": key_words, "total_nums": total_nums, "page_nums": page_nums, "last_seconds": last_seconds})
```



调试代码

```python
response = client.search(
    index="jobbole",
    body={
        "query":{
            "multi_match":{
                "query": keywords,
                "fields": ["tags", "title", "content"]
            }
        },
        "form":0,
        "size":10,
        "highlight": {
            "pre_tags": ['<span class="keyWord">'],
            "post_tags": ['</span>'],
            "fields": {
                "title": {},
                "content": {}
            }
        }
    }
)

# 此处打断点 total_nums = response["hits"]["total"]
total_nums = response["hits"]["total"]
```



**配置 html**

result.html

```html
{% load staticfiles %}

{% static 'css/style.css' %}
{% static 'js/global.js' %}

var search_url = "{% url 'search' %}"  
```

删除所有 `div class="resultItem"`，只保留一个即可。

```html
<div class="resultList">
    {% for hit in all_hits %}
    <div class="resultItem">
        <div class="itemHead">
            <a href="{{ hit.url }}" target="_blank" class="title">{% autoescape off %}{{ hit.title }}{% endautoescape %}</a>
            <span class="dependValue">
            	<span class="value">{{ hit.score }}</span>
            </span>
        </div>
        <div class="itemBody">
            {% autoescape off %}{{ hit.content}}{% endautoescape %}
        </div>
    </div>
    {% endfor %}
</div>
```



index.html

```html
var search_url = "{% url 'search' %}"
```



# 分页

result.html

```html
//分页
$(".pagination").pagination({{ total_nums }}), {
	current_page:{{ page|add:'-1'}},
	items_per_page: 10,
	display_msg:true,
	callback:pageselectCallback	
});
function pageselectCallback(page_id, jq){
	window.location.href=search_url+'?q'+key_words+'&p='+page_id
}

<div class="totalResult">{{ total_nums }} 
    class="time">{{ last_seconds }}
    class="totalPage">{{ page_nums }}
</div>

<p class="resultTotal">
                	<span class="info">找到约&nbsp;<span class="totalResult">{{ total_nums }}</span>&nbsp;条结果(用时<span class="time">{{ last_seconds }}</span>秒)，共约<span class="totalPage">{{ page_nums }}</span>页</span>
                </p>
```



search/views.py

```python
page = request.GET.get("p", 1)
try:
    page = int(page)
except:
    page = 1
return "page":page, "total_nums": total_nums
```



# 开发知识点

本地缓存，前端 js



add_search 事件

killRepeat 



## 网站 填充的内容

通过 redis 存储统计数据



```python
import redis


redis_cli = redis.StrictRedis(host="localhost")


def save_to_es(self):
    redis_cli.incr("jobbole_count")
```



调试代码

```python
import redis


redis_cli = redis.StrictRedis(host="localhost")
redis_cli.incr("jobbole_count")
```



redis-cli

```shell
keys *

type jobbole_count

get jobbole_count
```



views.py

```python
import redis

redis_cli = redis.StrictRedis()


jobbole_count = redis_cli.get("jobbole_count")

return "jobbole_count": jobbole_count
```



result.html

```html
网站
<span class="unit">{{ jobbole_count }}</span>

<div class="subfield">网站</div>
<ul class="subfieldContext">
    <li>
        <span class="name">伯乐在线</span>
        <span class="unit">({{ jobbole_count }})</span>
    </li>
```



## 热门搜索

redis 排序



ZINCRBY 为有序集 key 做增量加1。

ZREVERANGEBYSCORE



**配置 result.html**

views.py

```python
redis_cli.zincrby("search_keywords_set", key_words)

topn_search = redis_cli.zrevrangebyscore("search_keywords_set", "+inf", "-inf", start=0, num=5)

return "topn_search": topn_search
```



result.html

```html
ul class="historyList"
{% for search_word in topn_search %}
<li><a href="/search?q={{ search_word }}">{{ search_word }}</a></li>
{% endfor %}

<div class="hotSearch">
    <h6>热门搜索</h6>
    <ul class="historyList">
        {% for search_word in topn_search %}
        <li><a href="/search?q={{ search_word }}">{{ search_word }}</a></li>
        {% endfor %}
```



**配置 index.html** 

search/views.py

```python
class IndexView(View):
    
    # 首页
    def get(self, request):
        topn_search = redis_cli.zrevrangebyscore("search_keywords_set", "+inf", "-inf", start=0, num=5)
        return render(request, "index.html", {"topn_search": topn_search})
```



index.html

```html
class="history
{% for search_words in topn_search %}
	<a href="/search?q={{ search_words }}">{{ search_words }}</a>
{% endfor %}

<div class="historyArea">
    <p class="history">
        <label>热门搜索：</label>

    </p>
    <p class="history mysearch">
        <label>我的搜索：</label>
        {% for search_word in topn_search %}
        <span class="all-search">
            <a href="/search?q={{ search_word }}">{{ search_word }}</a>
        </span>
        {% endfor %}
```



LcvSearch/urls.py

```python
url(r'^$', IndexView.as_view(), name="index")
```



## 搜索类型

views.py

```python
SearchView():
	s_type = request.GET.get("s_type", "article")
```



github django-dynamic-scrapper



## 第12章 Scrapyd部署Scrapy爬虫
本章主要通过scrapyd完成对scrapy爬虫的线上部署。

## 第13章 课程总结
重新梳理一遍系统开发的整个过程， 让同学对系统和开发过程有一个更加直观的理解