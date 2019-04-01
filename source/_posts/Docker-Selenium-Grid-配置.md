---
title: Docker Selenium Grid 配置
date: 2018-10-26 12:27:37
tags:
---

## 通过 Docker 方式启动

[Docker images for Selenium Grid Server (Standalone, Hub, and Nodes)](https://github.com/seleniumHQ/docker-selenium)

拉取镜像

>查看版本信息 [docker-selenium](https://github.com/SeleniumHQ/docker-selenium/releases)

```shell
docker pull selenium/hub:3.14.0
```

```shell
docker pull selenium/node-chrome:3.14.0
```

启动主[hub](https://hub.docker.com/r/selenium/hub/tags/)

```shell
docker run -d -P --name selenium-hub selenium/hub:3.14.0
```

启动分组[node-chrome](https://hub.docker.com/r/selenium/node-chrome/tags/)

```shell
docker run -d --link selenium-hub:hub selenium/node-chrome:3.14.0
```

> --link 通过 link 关联 `selenium-hub` 容器，并为其设置了别名`hub`



## 通过 docker-compose 方式启动

docker-compose.yaml

```yaml
version: "3"
services:
  selenium-hub:
    image: selenium/hub:3.14.0-iron
    container_name: selenium-hub
    ports:
      - "4444:4444"
  chrome:
    image: selenium/node-chrome:3.14.0-iron
    depends_on:
      - selenium-hub
    environment:
      - HUB_HOST=selenium-hub
      - HUB_PORT=4444
  firefox:
    image: selenium/node-firefox:3.14.0-iron
    depends_on:
      - selenium-hub
    environment:
      - HUB_HOST=selenium-hub
      - HUB_PORT=4444
```



目前使用的

```yaml
# To execute this docker-compose yml file use `docker-compose -f <file_name> up`
# Add the `-d` flag at the end for detached execution
version: "3"
services:
  selenium-hub:
    image: selenium/hub:3.141.59-lithium
    # container_name: selenium-hub
    environment:
      - GRID_MAX_SESSION=10
      # - newSessionWaitTimeout=25000
      - JAVA_OPTS=-Xmx512m
      # - SE_OPTS="-debug"
    ports:
      - "4444:4444"
  chrome:
    image: selenium/node-chrome-debug:3.141.59-lithium
    volumes:
      - /dev/shm:/dev/shm
    depends_on:
      - selenium-hub
    environment:
      - HUB_HOST=selenium-hub
      - HUB_PORT=4444
      - NODE_MAX_INSTANCES=10
      - NODE_MAX_SESSION=10
      - SCREEN_WIDTH=1366
      - SCREEN_HEIGHT=768 
      - SCREEN_DEPTH=24
    ports:
      - "5900:5900"
  firefox:
    image: selenium/node-firefox-debug:3.141.59-lithium
    volumes:
      - /dev/shm:/dev/shm
    depends_on:
      - selenium-hub
    environment:
      - HUB_HOST=selenium-hub
      - HUB_PORT=4444
      - NODE_MAX_INSTANCES=10
      - NODE_MAX_SESSION=10
      - SCREEN_WIDTH=1366
      - SCREEN_HEIGHT=768 
      - SCREEN_DEPTH=24
    ports:
      - "5901:5900"
```

vnc 密码：`secret`



## 踩坑记

### 1.Docker selenium 中文乱码，未验证

> 3.141.59-lithium 这个版本打开 gbk 编码的网站没有问题，例如：“163.com”

Dockerfile

```
FROM selenium/node-chrome-debug

USER root
 
RUN apt-get update \
    && apt-get -y install ttf-wqy-microhei ttf-wqy-zenhei \
    && apt-get clean
```



构建

```
docker build -t selenium/node-chrome-debug-zh-cn .
```



### 2. 窗口最大化失败

在脚本中对浏览器进行最大化操作：`driver.maximize_window()`这个命令一向运行是没问题的，但是在docker 中却报错如下：
Message: unknown error: failed to change window state to maximized, current state is normal

#### 原因：docker 运行 node 没有设置屏幕尺寸。

```
- SCREEN_WIDTH=1366
- SCREEN_HEIGHT=768 
- SCREEN_DEPTH=24
```

#### 或者：

查了一下，说是selenium 的bug。 找了一下，没有合适的解决方案，粗暴解决如下：

```python
try:
    driver.maximize_window()
except WebDriverException as e:
    log.log().logger.info(e)
    driver.set_window_size(1920, 1080)  #如果最大化失败，设置窗口大小为 1920*1080
```



### 3. chrome option 不生效。

因为部分用例需要模拟移动设备，或设置浏览器为英文，所以使用 chrome option进行设置。 原来的初始化脚本如下：

```python
desired_caps_web = webdriver.DesiredCapabilities.CHROME
deviceList = ['Galaxy S5', 'Nexus 5X', 'Nexus 6P', 'iPhone 6', 'iPhone 6 Plus', 'iPad', 'iPad Pro']
if devicename!='' :
    if devicename not in deviceList:
        devicename = deviceList[2]
    chrome_option = {
        'args': ['lang=en_US','start-maximized'],
        'extensions': [], 'mobileEmulation': {'deviceName': ''}

    }
    chrome_option['mobileEmulation']['deviceName'] = devicename
else:
    chrome_option = {
        'args': ['lang=en_US','--start-maximized'],
        'extensions': []
    }
desired_caps_web['goog:chromeOptions']=chrome_option
log.log().logger.info(desired_caps_web)
driver = webdriver.remote.webdriver.WebDriver(command_executor=server_url,desired_capabilities=desired_caps_web)
```

但同样，之前一直正常运行的脚本，到 docker 里不起作用。
看下docker selenium node 节点的log ，发现打印了如下信息：

```python
Capabilities are: Capabilities {browserName: chrome, chromeOptions: {args: [lang=zh_CN.UTF-8],  mobileEmulation: {deviceName: iPhone 6}}, goog:chromeOptions: {}, javascriptEnabled: true, version: }
```

多了个 goog:chromeOptions {} 的配置项是怎么回事？

认真看下，Capabilities 里我设置的 chromeOptions 已经正确传进来了，但是后面的 goog:chromeOptions: {} 似乎覆盖了对应的配置。
尝试下把脚本里的参数名称从 “chromeOptions ” 改为 “goog:chromeOptions” ，奇迹出现了：

```python
Capabilities are: Capabilities {browserName: chrome, goog:chromeOptions: {args: [lang=zh_CN.UTF-8], mobileEmulation: {deviceName: iPhone 6}}, javascriptEnabled: true, version: }
```

脚本也能正常运行了，对应的浏览器语言、移动设备模拟设置也已生效！

于是修改对应脚本为：

```python
desired_caps_web['goog:chromeOptions']=chrome_option
```

问题解决！



## 参考

[SeleniumHQ/selenium](https://github.com/SeleniumHQ/selenium)

[SeleniumHQ/docker-selenium](https://github.com/SeleniumHQ/docker-selenium)

[Selenium Conference](https://www.seleniumconf.com/)



[docker+selenium+python构建前端自动化分布式测试环境](https://yeqown.github.io/2018/01/23/docker-selenium-python%E6%9E%84%E5%BB%BA%E5%89%8D%E7%AB%AF%E8%87%AA%E5%8A%A8%E5%8C%96%E5%88%86%E5%B8%83%E5%BC%8F%E6%B5%8B%E8%AF%95%E7%8E%AF%E5%A2%83/)

[Docker Selenium-虫师](https://www.cnblogs.com/fnng/p/8358326.html)

[docker+selenium grid+python实现分布式自动化测试](https://blog.csdn.net/songer_xing/article/details/78626592): 解决中文乱码问题

[docker+selenium 搭建和踩坑记录](https://testerhome.com/topics/13372)

