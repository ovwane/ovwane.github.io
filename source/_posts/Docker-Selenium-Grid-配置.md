---
title: Docker Selenium Grid 配置
date: 2018-10-26 12:27:37
tags:
---

[Docker images for Selenium Grid Server (Standalone, Hub, and Nodes)](https://github.com/seleniumHQ/docker-selenium)

拉取镜像

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



## 参考

[docker+selenium+python构建前端自动化分布式测试环境](https://yeqown.github.io/2018/01/23/docker-selenium-python%E6%9E%84%E5%BB%BA%E5%89%8D%E7%AB%AF%E8%87%AA%E5%8A%A8%E5%8C%96%E5%88%86%E5%B8%83%E5%BC%8F%E6%B5%8B%E8%AF%95%E7%8E%AF%E5%A2%83/)

[Docker Selenium-虫师](https://www.cnblogs.com/fnng/p/8358326.html)

[docker+selenium grid+python实现分布式自动化测试](https://blog.csdn.net/songer_xing/article/details/78626592)

