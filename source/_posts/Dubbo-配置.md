---
title: Dubbo 配置
date: 2019-01-16 14:48:30
tags:
---

# [Dubbo](https://dubbo.apache.org)

1. Zookeeper
2. Dubbo-admin

> dubbo 2.7.0
>
> dubbo-admin 0.2.0



## [Zookeeper](https://zookeeper.apache.org/)

[install Zookeeper configuration center](https://dubbo.apache.org/en-us/docs/admin/install/zookeeper.html)

```shell
docker pull zookeeper:3.5.6
```



运行 Zookeeper

```shell
docker run -d --name zookeeper-3.5.6 -p 2181:2181 zookeeper:3.5.6
```



测试

```shell
echo dump | nc 127.0.0.1 2181
```



## [Dubbo-admin](https://github.com/apache/dubbo-admin)

[apache/dubbo-admin](https://hub.docker.com/r/apache/dubbo-admin)

```
docker pull apache/dubbo-admin:latest
```

运行

```
docker run -d --name dubbo-admin -p 8080:8080 --link zookeeper-3.5.6:zookeeper -e admin.registry.address=zookeeper://zookeeper:2181 -e admin.config-center=zookeeper://zookeeper:2181 -e admin.metadata-report.address=zookeeper://zookeeper:2181 dubbo-admin:latest
```



### 单独构建

Dockerfile

```dockerfile
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM openjdk:8-jdk
RUN mkdir /source && wget https://github.com/apache/dubbo-admin/archive/0.2.0.zip && unzip 0.2.0.zip -d /source
WORKDIR /source/dubbo-admin-0.2.0
# 使用 settings.xml 的原因是要使用阿里的 maven 源。
ADD settings.xml /source/dubbo-admin-0.2.0
RUN ./mvnw clean package --settings settings.xml -Dmaven.test.skip=true

FROM openjdk:8-jre
LABEL maintainer="dev@dubbo.apache.org"
COPY --from=0 /source/dubbo-admin-0.2.0/dubbo-admin-distribution/target/dubbo-admin-0.2.0.jar /app.jar
ENTRYPOINT ["java","-XX:+UnlockExperimentalVMOptions","-XX:+UseCGroupMemoryLimitForHeap","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
EXPOSE 8080
```



settings.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
  <mirrors>
    <mirror>
        <id>aliyunmaven</id>
        <mirrorOf>*</mirrorOf>
        <name>阿里云公共仓库</name>
        <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
  </mirrors>
</settings>
```



构建

```
docker build -t dubbo-admin:0.2.0 .
```



运行

```shell
docker run -d --name dubbo-admin -p 8080:8080 --link zookeeper-3.5.6:zookeeper -e admin.registry.address=zookeeper://zookeeper:2181 -e admin.config-center=zookeeper://zookeeper:2181 -e admin.metadata-report.address=zookeeper://zookeeper:2181 dubbo-admin:0.2.0
```



访问

http://IP:8080   用户名：`root`，密码：`root`



## 运行 zookeeper 和 dubbo-admin

docker-compose.yml

```yml
version: '3'

services:
  zookeeper:
    image: zookeeper:3.5.6
    ports:
      - 2181:2181
  admin:
    image: apache/dubbo-admin
    depends_on:
      - zookeeper
    ports:
      - 8080
    environment:
      - admin.registry.address=zookeeper://zookeeper:2181
      - admin.config-center=zookeeper://zookeeper:2181
      - admin.metadata-report.address=zookeeper://zookeeper:2181
```



## Dubbo

**安装 JDK**

**安装 Maven**

**下载示例**

> Dubbo 2.7.0
>
> pom.xml 依赖可以更改一下

```shell
git clone https://github.com/apache/dubbo-samples.git
```

```shell
cd dubbo-samples/java
mvn clean package
```



### Dubbo Provider

```shell
cd dubbo-samples-basic

mvn -Djava.net.preferIPv4Stack=true \
-Dexec.mainClass=org.apache.dubbo.samples.basic.BasicProvider \
exec:java
```



### Dubbo Consumer

```shell
cd dubbo-samples-basic

mvn -Djava.net.preferIPv4Stack=true \
-Dexec.mainClass=org.apache.dubbo.samples.basic.BasicConsumer \
exec:java
```



## 参考

[Docker多步构建生成dubbo-admin镜像](https://www.huangyunkun.com/2018/04/19/docker-multi-step-dubbo-admin/)

[使用Docker容器化SpringBoot+Dubbo应用的实践](https://luoliangdsga.github.io/2018/06/10/%E4%BD%BF%E7%94%A8Docker%E5%AE%B9%E5%99%A8%E5%8C%96SpringBoot-Dubbo%E5%BA%94%E7%94%A8%E7%9A%84%E5%AE%9E%E8%B7%B5/)

[基于SpringBoot+Dubbo的微服务框架（借助Docker+Jenkins实现自动化、容器化部署）](https://github.com/bz51/SpringBoot-Dubbo-Docker-Jenkins)

[Dubbo - Dubbo Admin 安装（生产版）](https://blog.csdn.net/u012627861/article/details/82945068)

[Dubbo - Dubbo Admin 安装（开发版-Dubbo OPS）](https://blog.csdn.net/u012627861/article/details/82754027)

[dubbo入门学习笔记之环境准备](https://www.cnblogs.com/darling2047/p/9681181.html)

 [dubbo提供者注册容器IP问题 | Vnimos's blog](https://vnimos.cn/2017/07/11/dubbo-provider-in-docker/) 