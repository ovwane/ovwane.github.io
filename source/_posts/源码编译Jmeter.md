---
title: 源码编译Jmeter
date: 2018-10-09 08:19:39
tags:
---



安装Ant工具

```shell
brew install ant
```



下载JMeter源码

```shell
git clone https://github.com/apache/jmeter.git
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

