---
title: Docker运行MariaDB
date: 2018-09-01 11:25:39
tags:
---

```shell
docker pull mariadb:10.3.9

docker run -p 3307:3306 -v ~/docker/mariadb1:/var/lib/mysql -e TIMEZONE=Asis/Shanghai -e MYSQL_ROOT_PASSWORD=root -e SERVER_ID=1 --name mariadb-10.3.9-1 -d mariadb:10.3.9
```

## 参考

[docker安装配置Mariadb数据库](https://www.centos.bz/2017/07/docker-install-mariadb/)