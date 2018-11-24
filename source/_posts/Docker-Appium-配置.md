---
title: Docker Appium 配置
date: 2018-10-26 12:57:52
tags:
---

拉取镜像

[appium](https://hub.docker.com/r/appium/appium/tags/)

```shell
docker pull appium/appium:1.9.1-p0
```

[appium-emulator](https://hub.docker.com/r/appium/appium-emulator/tags/)

```shell
docker pull appium/appium-emulator:1.1-arsenal
```

运行

```shell
docker run --privileged -d -p 4723:4723  -v /dev/bus/usb:/dev/bus/usb --name appium-1.9.1-p0 appium/appium:1.9.1-p0
```



## 参考

[基于Docker配置Appium实践](https://www.pilipala195.com/2017/11/27/%E5%9F%BA%E4%BA%8EDocker%E9%85%8D%E7%BD%AEAppium%E5%AE%9E%E8%B7%B5/)

[Appium Docker for Android](https://github.com/appium/appium-docker-android)