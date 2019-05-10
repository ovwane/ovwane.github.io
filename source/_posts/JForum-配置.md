---
title: JForum 配置
date: 2019-04-01 22:26:57
tags:
---



## 启动 MySQL

```shell
docker pull mariadb:10.3.9
```

```shell
docker run -p 3306:3306 -v ~/docker/mariadb:/var/lib/mysql -e TIMEZONE=Asis/Shanghai -e MYSQL_ROOT_PASSWORD=root -e SERVER_ID=1 --name mariadb-10.3.9 -d mariadb:10.3.9
```



连接数据库

```shell
mysql -h 127.0.0.1 -P 3306 -uroot -p
```



新建 jforum 数据库

```mysql
CREATE DATABASE jforum DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
```



## 启动 Tomcat

```shell
docker pull tomcat:8.5.39-alpine
```

```shell
docker run -d --name jforum_tomcat -p 8080:8080  -v ~/docker/tomcat/webapps:/usr/local/tomcat/webapps tomcat:8.5.39-alpine
```



下载 JForum 并复制 JForum 文件到 ~/docker/tomcat/webapps 目录下

```shell
cp -r jforum ~/docker/tomcat/webapps
```



修改 jforum 连接 mysql 的用户名和密码

```shell
jforum/WEB-INF/config/database/mysql/mysql.properties
```



修改数据库引擎，查找所有的 “TYPE=InnoDB” 修改为 “ENGINE=InnoDB”。

```shell
jforum\WEB-INF\config\database\mysql\mysql_db_struct.sql
```



```shell
docker build -t jforum:2.1.9 .
```



保存镜像

```shell
docker save jforum:2.1.9 -o jforum:2.1.9.tar
```



加载镜像

> 可以在任何装 docker 的地方加载 刚保存的镜像了。

```shell
docker load -i jforum:2.1.9.tar
```



启动 jforum

```shell
docker run -d --name jforum-2.1.9 -p 8080:8080 --link mariadb-10.3.9:mysql jforum:2.1.9
```



配置 jforum ，浏览器访问

http://localhost:8080/jforum/install.jsp



## 参考

[JForum论坛安装以及部署](<https://blog.csdn.net/jhyfugug/article/details/79467369>)