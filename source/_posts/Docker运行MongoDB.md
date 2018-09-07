---
title: Docker运行MongoDB
date: 2018-09-01 11:13:13
tags:
---

创建并运行MongoDB镜像

```shell
docker pull mongo:4.1.2

docker run -p 27017:27017 -v ~/docker/mongodb:/data/db --name mongodb-4.1.2 -d mongo:4.1.2
```

## 参考

[Docker MongoDB 部署](https://www.jianshu.com/p/6fdb2bcb4b43)