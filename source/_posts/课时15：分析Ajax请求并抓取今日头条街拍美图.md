---
title: 课时15：分析Ajax请求并抓取今日头条街拍美图
date: 2018-07-31 20:35:17
tags:
---

爬取起始页面 

https://www.toutiao.com/search/?keyword=%E8%A1%97%E6%8B%8D



抓取索引页内容

抓取详情页内容

下载图片与保存数据库

开启循环及多线程



项目 Jeipai

import requests

def get_page_index()