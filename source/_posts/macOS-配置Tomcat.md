---
title: macOS 配置Tomcat
date: 2018-10-15 11:24:32
tags:
---

#### 安装Apr

```shell
brew install apr apr-util
```

#### 安装Tomcat

```shell
brew install tomcat
```

#### 启动Tomcat

```shell
brew services start tomcat
brew services restart tomcat
```

#### 配置文件目录

```shell
cd /usr/local/Cellar/tomcat/
cd /usr/local/Cellar/tomcat/9.0.12/libexec/conf
```

#### 部署jar项目

```shell
cp jenkins.jar /usr/local/Cellar/tomcat/9.0.12/libexec/webapps 
```

#### 监控服务器

1、确保Tomcat可以正常启动

2、打开conf下的tomcat-users.xml并添加
```xml
<role rolename="manager-gui"/>
<user username="admin" password="admin" roles="manager-gui"/>
```

3、重启tomcat并访问[http://localhost:8080/manager/html](http://localhost:8080/manager/html)

4、[http://localhost:8080/manager/status](http://localhost:8080/manager/status)