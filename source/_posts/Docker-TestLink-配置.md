---
title: Docker TestLink 配置
date: 2019-01-22 10:00:32
tags:
---

# [bitnami/testlink](https://hub.docker.com/r/bitnami/testlink)

```
mkdir -p /data/docker/testlink
chmod 777 /data/docker/testlink
```

vim docker-compose.yml

```yaml
version: '3'

services:
  mariadb:
    image: 'bitnami/mariadb:latest'
    environment:
      - MARIADB_USER=bn_testlink
      - MARIADB_DATABASE=bitnami_testlink
      - MARIADB_ROOT_PASSWORD=testlink
      - MARIADB_PASSWORD=testlink
    volumes:
      - '/data/docker/testlink/mariadb:/bitnami'
      
  testlink:
    image: 'bitnami/testlink:latest'
    environment:
      - TESTLINK_DATABASE_USER=bn_testlink
      - TESTLINK_DATABASE_NAME=bitnami_testlink
      - TESTLINK_DATABASE_PASSWORD=testlink
    ports:
      - '80:80'
      - '443:443'
    volumes:
      - '/data/docker/testlink/testlink:/bitnami'
    depends_on:
      - mariadb
```

```shell
docker-compose up -d
```

### 访问

http://IP

- Default user: `user`
- Default password: `bitnami`