---
title: Docker 运行Zentao
date: 2018-09-08 13:19:09
tags:
---

```shell
docker pull haha123/zentao:latest # zentao 9.2.1
```

```shell
docker run  -p 8800:80 -v ~/docker/zentao:/opt/zbox -d --name zentao-9.2.1 --restart=always haha123/zentao:latest
```

访问：

默认user/passwd: admin/123456

## 参考

[haha123](https://hub.docker.com/u/haha123/)/[zentao](https://hub.docker.com/r/haha123/zentao/)