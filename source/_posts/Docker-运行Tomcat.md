---
title: Docker 运行Tomcat
date: 2018-10-08 19:59:13
tags:
---



```shell
docker pull tomcat:8.5.34-jre8-alpine
```

```shell
docker run  -p 8803:8080 -v ~/docker/tomcat:/usr/local/tomcat/webapps -d --name tomcat-8.5.34 --restart=always tomcat:8.5.34-jre8-alpine
```

