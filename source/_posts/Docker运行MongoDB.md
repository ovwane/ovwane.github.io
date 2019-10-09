---
title: Docker运行MongoDB
date: 2018-09-01 11:13:13
tags:
---

# # [MongoDB](https://github.com/mongodb/mongo)

 [mongo](https://hub.docker.com/_/mongo)

拉取MongoDB镜像

```shell
docker pull mongo:4.2.0
```



运行

```
docker run -d --name mongodb-4.2.0 -p 27017:27017 -v ~/docker/mongodb:/data/db mongo:4.2.0
```



## 参考

[Docker MongoDB 部署](https://www.jianshu.com/p/6fdb2bcb4b43)