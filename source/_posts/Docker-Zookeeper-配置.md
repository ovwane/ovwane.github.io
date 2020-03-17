---
title: Docker Zookeeper 配置
date: 2020-01-15 17:13:25
tags:
---

[Zookeeper](https://hub.docker.com/_/zookeeper)



```
docker pull zookeeper:3.5.6
```

```
docker run -d --name zookeeper-3.5.6 -p 2181:2181 -p 28080:8080 -p 2888:2888 -p 3888:3888 -v ~/docker/zookeeper/zoo.cfg:/conf/zoo.cfg zookeeper:3.5.6
```

