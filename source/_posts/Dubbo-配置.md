---
title: Dubbo 配置
date: 2019-01-16 14:48:30
tags:
---

# [Dubbo](https://dubbo.apache.org)

> Ubuntu 18.04
>
> Zookeeper 3.3.3

## [Zookeeper](https://zookeeper.apache.org/) 配置

[install Zookeeper configuration center](https://dubbo.apache.org/en-us/docs/admin/install/zookeeper.html)

```shell
docker pull zookeeper:3.3.6
```

> Docker 没有 Zookeeper 3.3.3 版本。
>
> Zookeeper 3.5 是因为 Java Zookeepeer 提示 old server。rr-w 不生效。

```shell
docker pull zookeeper:3.5
```

**运行** **Zookeeper**

```shell
docker run -d --name zookeeper-3.3.6 -p 2181:2181 zookeeper:3.3.6
```

```shell
docker run -d --name zookeeper-3.5 -p 2181:2181 zookeeper:3.5
```

**测试**

```shell
echo dump | nc 127.0.0.1 2181
```

## [Dubbo ops](https://github.com/apache/dubbo-ops)

```shell
docker pull riveryang/dubbo-admin
```

```
docker run -d --name dubbo-admin -p 8080:8080 --link zookeeper-3.3.6:zk -e DUBBO_REGISTRY="zookeeper:\/\/zk:2181" riveryang/dubbo-admin
```

```shell
docker run -d --name dubbo-admin -p 8080:8080 --link zookeeper-3.5:zk -e DUBBO_REGISTRY="zookeeper:\/\/zk:2181" riveryang/dubbo-admin
```

**访问**

http://IP:8080

用户名：`root`，密码：`root`

## Dubbo

**安装 JDK**

**安装 Maven**

**下载示例**

```shell
git clone https://github.com/apache/dubbo-samples.git
```

```shell
cd dubbo-samples
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

### ~~[Dubbo ops](https://github.com/apache/dubbo-ops)~~

```shell
docker pull riveryang/dubbo-admin
```

```
docker run -d --name dubbo-admin -p 8080:8080 --link zookeeper-3.3.6:zk -e DUBBO_REGISTRY="zookeeper:\/\/zk:2181" riveryang/dubbo-admin
```

~~下载~~

```shell
git clone https://github.com/apache/dubbo-ops.git
cd incubator-dubbo-ops/
```

```shell
vim dubbo-admin-backend/src/main/resources/application.properties
```



~~修改 Zookeeper IP~~

```shell
vim dubbo-admin-backend/src/main/resources/application.properties
```

~~安装npm~~

```shell
apt install -y npm
npm i npm@latest -g
npm -v
```

```
cd dubbo-admin-frontend/
npm install ajv
```

~~Build~~

```shell
mvn clean package
```

~~启动~~

```shell
mvn --projects dubbo-admin-backend spring-boot:run
```

~~or~~

```shell
cd dubbo-admin-backend/target; java -jar dubbo-admin-backend-0.0.1-SNAPSHOT.jar
```

## 参考

[Docker多步构建生成dubbo-admin镜像](https://www.huangyunkun.com/2018/04/19/docker-multi-step-dubbo-admin/)

[使用Docker容器化SpringBoot+Dubbo应用的实践](https://luoliangdsga.github.io/2018/06/10/%E4%BD%BF%E7%94%A8Docker%E5%AE%B9%E5%99%A8%E5%8C%96SpringBoot-Dubbo%E5%BA%94%E7%94%A8%E7%9A%84%E5%AE%9E%E8%B7%B5/)

[基于SpringBoot+Dubbo的微服务框架（借助Docker+Jenkins实现自动化、容器化部署）](https://github.com/bz51/SpringBoot-Dubbo-Docker-Jenkins)

[Dubbo - Dubbo Admin 安装（生产版）](https://blog.csdn.net/u012627861/article/details/82945068)

[Dubbo - Dubbo Admin 安装（开发版-Dubbo OPS）](https://blog.csdn.net/u012627861/article/details/82754027)

[dubbo入门学习笔记之环境准备](https://www.cnblogs.com/darling2047/p/9681181.html)