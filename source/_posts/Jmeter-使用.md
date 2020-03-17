---
title: Jmeter 使用
date: 2018-10-09 08:19:39
tags:
---

## 编译 [JMeter](https://github.com/apache/jmeter)

安装Ant工具

```shell
brew install ant
```



下载JMeter源码

```shell
git clone -b v5_1_1 https://github.com/apache/jmeter.git jmeter/v5.1.1
```



ant初始化build环境

```shell
ant download_jars
```

执行下ant

```shell
ant
```

build，This will compile the application and enable you to run `jmeter` from the `bin` directory.

```shell
ant test [-Djava.awt.headless=true]
```



## [插件管理器](https://jmeter-plugins.org/install/Install/)

下载 `plugins-manager.jar` 放到 JMeter 主目录的  `lib/ext`目录下，然后重新启动 JMeter。



### [JMeter WebSocket Samplers](https://bitbucket.org/pjtr/jmeter-websocket-samplers/src/master/)





## 参考

 [Apache JMeter - Building and Contributing to JMeter](https://jmeter.apache.org/building.html) 

[User's Manual](https://jmeter.apache.org/usermanual/index.html)

 [IDEA源码编译Jmeter 5.1.1_Jmeter_Lupu's journey - 生活、分享-CSDN博客](https://blog.csdn.net/u011436666/article/details/89198959) 