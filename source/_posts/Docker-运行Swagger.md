---
title: Docker 运行Swagger
date: 2018-10-07 09:29:15
tags:
---

# [Swagger](https://swagger.io/)

下载所需要的镜像

```shell
docker pull swaggerapi/swagger-ui:v3.19.2 &&\
docker pull swaggerapi/swagger-editor:v3.6.11
```



运行 swagger-ui

```shell
docker run -p 8801:8080 -d --name swagger-ui-3.19.2 swaggerapi/swagger-ui:v3.19.2
```

For `latest`, and post `v3.0.5` tags, you can specify the default swagger-ui to load in the UI:

```shell
docker run -p 80:8080 -e API_URL=http://generator.swagger.io/api/swagger.json swaggerapi/swagger-ui:v3.19.2
```



运行 swagger-editor

```shell
docker run -d -p 8802:8080 -d --name swagger-editor-3.6.11 swaggerapi/swagger-editor:v3.6.11
```



访问

http://IP:8080



## 参考

[Docker Hub swaggerapi/swagger-ui](https://hub.docker.com/r/swaggerapi/swagger-ui/)

[Docker Hub swaggerapi/swagger-editor](https://hub.docker.com/r/swaggerapi/swagger-editor/)



[docker部署swagger](https://blog.csdn.net/SweetyoYY/article/details/78388524)

[DIY搭建Swagger-UI](https://o-my-chenjian.com/2017/04/19/Play-API-With-Swagger/)