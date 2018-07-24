---
title: CentOS7 PhantomJS安装配置
date: 2017-07-25 19:02:18
---
[PhantomJS](http://phantomjs.org/)

#### 安装 PhantomJS
brew install phantomjs

版本

```
phantomjs --version
2.1.1
```

### CentOS 7.3 配置方法
#### 下载 PhantomJS
http://phantomjs.org/download.html
```
https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-2.1.1-macosx.zip
```

配置 PhantomJS

### [Selenium](http://www.seleniumhq.org/)
#### 安装 Selenium
pip3 install selenium

依赖

```

```

#### 下载 ChromeDriver - WebDriver for Chrome
[ChromeDriver](https://sites.google.com/a/chromium.org/chromedriver/)

##### 配置 
cp chromedriver /usr/local/bin/

##### 查看版本
```
chromedriver -V
Starting ChromeDriver 2.31.488774 (7e15618d1bf16df8bf0ecf2914ed1964a387ba0b) on port 9515
Only local connections are allowed.
```

##### 测试
```
from selenium import webdriver

browser = webdriver.Chrome()
browser.get('http://www.baidu.com/')
```
