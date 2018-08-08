---
date: 2017-05-24 16:54:22
---

爬取策略的深度优先和广度优先

爬虫网址去重策略

基础环境

>Python 3.6.5  </br>
>PyCharm 2017.3.4 </br>
>MariaDB </br>
>Navicat </br>

`pip install scrapy`

命令行创建scrapy项目

`scrapy startproject ArticleSpider`

scrapy目录结构

>scrapy.cfg

配置文件

>setings.py
设置
SPIDER_MODULES = ['ArticleSpider.spiders'] #存放spider的路径
NEWSPIDER_MODULE = 'ArticleSpider.spiders'
pipelines.py:

做跟数据存储相关的东西

>middilewares.py:

自己定义的middlewares 定义方法，处理响应的IO操作

>init.py:

项目的初始化文件。

>items.py：

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

