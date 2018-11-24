---
title: Docker disconf配置
date: 2018-10-24 14:44:28
tags:
---

**新建 docker-compose 文件**

```shell
mkdir ~/docker/disconf

vim docker-compose.yaml
```

docker-compose.yaml

```yaml
version: '3'
services:
  disconf_redis_1: 
    image: daocloud.io/library/redis
    restart: always
  disconf_redis_2: 
    image: daocloud.io/library/redis
    restart: always
  disconf_zookeeper: 
    image: zookeeper:3.3.6
    restart: always
  disconf_mysql: 
    image: bolingcavalry/disconf_mysql:0.0.1
    environment:
      MYSQL_ROOT_PASSWORD: 123456
    restart: always
  disconf_tomcat: 
    image: bolingcavalry/disconf_tomcat:0.0.1
    links: 
      - disconf_redis_1:redishost001 
      - disconf_redis_2:redishost002
      - disconf_zookeeper:zkhost
      - disconf_mysql:mysqlhost
    restart: always
  disconf_nginx: 
    image: bolingcavalry/disconf_nginx:0.0.1
    links: 
      - disconf_tomcat:tomcathost 
    ports: 
      - "8820:80" 
    restart: always
```

**运行**

```shell
docker-compose up -d
```

停止整个环境的命令：

```
docker-compose stop1
```

删除整个环境的命令：

```
docker-compose rm
```

**访问**

用户名密码都是admin



## 参考

[Docker搭建disconf环境，三部曲之一：极速搭建disconf](https://blog.csdn.net/boling_cavalry/article/details/71082610)

[Docker搭建disconf环境，三部曲之二：本地快速构建disconf镜像](https://blog.csdn.net/boling_cavalry/article/details/71107498)

[Docker搭建disconf环境，三部曲之三：细说搭建过程](https://blog.csdn.net/boling_cavalry/article/details/71120725)