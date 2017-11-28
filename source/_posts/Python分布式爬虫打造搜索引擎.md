title: Python分布式爬虫打造搜索引擎笔记
date: 2017-10-18 22:37:00
categories:
- 技术
- Python
tags:
- Requests
- Scrapy
- Python
- Django
- Elasticsearch
---
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

## 第6章 通过CrawlSpider对招聘网站进行整站爬取
本章完成招聘网站职位的数据表结构设计，并通过link extractor和rule的形式并配置CrawlSpider完成招聘网站所有职位的爬取，本章也会从源码的角度来分析CrawlSpider让大家对CrawlSpider有深入的理解。

## 第7章 Scrapy突破反爬虫的限制
本章会从爬虫和反爬虫的斗争过程开始讲解，然后讲解scrapy的原理，然后通过随机切换user-agent和设置scrapy的ip代理的方式完成突破反爬虫的各种限制。本章也会详细介绍httpresponse和httprequest来详细的分析scrapy的功能，最后会通过云打码平台来完成在线验证码识别以及禁用cookie和访问频率来降低爬虫被屏蔽的可能性。...

## 第8章 Scrapy进阶开发
本章将讲解scrapy的更多高级特性，这些高级特性包括通过selenium和phantomjs实现动态网站数据的爬取以及将这二者集成到scrapy中、scrapy信号、自定义中间件、暂停和启动scrapy爬虫、scrapy的核心api、scrapy的telnet、scrapy的web service和scrapy的log配置和email发送等。 这些特性使得我们不仅只是可以通过scrapy来完成...

## 第9章 Scrapy-redis分布式爬虫
Scrapy-redis分布式爬虫的使用以及scrapy-redis的分布式爬虫的源码分析， 让大家可以根据自己的需求来修改源码以满足自己的需求。最后也会讲解如何将bloomfilter集成到scrapy-redis中。

## 第10章 Elasticsearch搜索引擎的使用
本章将讲解elasticsearch的安装和使用，将讲解elasticsearch的基本概念的介绍以及api的使用。本章也会讲解搜索引擎的原理并讲解elasticsearch-dsl的使用，最后讲解如何通过scrapy的pipeline将数据保存到elasticsearch中。

## 第11章 Django搭建搜索网站
本章讲解如何通过django快速搭建搜索网站， 本章也会讲解如何完成django与elasticsearch的搜索查询交互。

## 第12章 Scrapyd部署Scrapy爬虫
本章主要通过scrapyd完成对scrapy爬虫的线上部署。

## 第13章 课程总结
重新梳理一遍系统开发的整个过程， 让同学对系统和开发过程有一个更加直观的理解