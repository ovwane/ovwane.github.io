---
title: Docker 配置 Jira
date: 2018-09-05 20:29:49
tags:
---



## Jira

### 下载镜像

```
docker pull mysql:5.7.29
docker pull atlassian/jira-software:8.5.0
```



###  配置 MySQL 数据库

jira.cnf

```
[mysqld]
bind-address = 0.0.0.0
default-storage-engine=INNODB
character_set_server=utf8mb4
innodb_default_row_format=DYNAMIC
innodb_large_prefix=ON
# default value:  innodb_file_format=Barracuda
innodb_log_file_size=2G

[client]
default-character-set=utf8

[mysql]
default-character-set=utf8
```



jira.sql

```mysql
DROP DATABASE IF EXISTS `jira`;

CREATE DATABASE jira CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;

GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,REFERENCES,ALTER,INDEX on jira.* TO 'jira'@'%' IDENTIFIED BY 'jira';

flush privileges;
```



运行数据库

```shell
docker run -d --name jira-mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=root -v ${PWD}/Shanghai:/etc/localtime:ro -v ${PWD}/jira.cnf:/etc/mysql/conf.d/jira.cnf -v ${PWD}/jira.sql:/docker-entrypoint-initdb.d/jira.sql mysql:5.7.29
```



### 配置 Jira

[Supported platforms](https://confluence.atlassian.com/adminjiraserver085/supported-platforms-981154553.html)

下载  [MySQL Connector/J 5.1 driver](https://dev.mysql.com/downloads/connector/j/5.1.html)

[Jira 连接 MySQL 参数](https://confluence.atlassian.com/adminjiraserver085/connecting-jira-applications-to-mysql-5-7-981154582.html#ConnectingJiraapplicationstoMySQL5.7-dbconnectionfields)



jira.env

```
'ATL_JDBC_URL=jdbc:mysql://address=(protocol=tcp)(host=mysql)(port=3306)/jiradb?useUnicode=true&amp;characterEncoding=UTF8&amp;sessionVariables=default_storage_engine=InnoDB&amp;useSSL=false'
'ATL_JDBC_USER=jira'
'ATL_JDBC_PASSWORD=jira'
'ATL_DB_DRIVER=com.mysql.jdbc.Driver'
'ATL_DB_TYPE=mysql'
'ATL_DB_SCHEMA_NAME=public'
```



运行 Jira

```
docker run -d --name jira --link jira-mysql:mysql -p 8080:8080 -e JVM_SUPPORT_RECOMMENDED_ARGS="-Duser.timezone=Asia/Shanghai" -e JVM_MINIMUM_MEMORY=2048m -e JVM_MAXIMUM_MEMORY=4096m -v ${PWD}/mysql-connector-java-5.1.48-bin.jar:/opt/atlassian/jira/lib/mysql-connector-java-5.1.48-bin.jar atlassian/jira-software:8.5.0
```

```
docker run -d --name jira1 --link mysql:mysql -p 8081:8080 -e JVM_SUPPORT_RECOMMENDED_ARGS="-Duser.timezone=Asia/Shanghai" -e JVM_MINIMUM_MEMORY=2048m -e JVM_MAXIMUM_MEMORY=4096m -v ${PWD}/mysql-connector-java-5.1.48-bin.jar:/opt/atlassian/jira/lib/mysql-connector-java-5.1.48-bin.jar --env-file=${PWD}/jira.env atlassian/jira-software:8.5.0
```



## 源码构建

```shell
git clone https://github.com/cptactionhank/docker-atlassian-jira.git

cd docker-atlassian-jira
```



Dockerfile

```dockerfile
FROM openjdk:8

# Configuration variables.
ENV JIRA_HOME     /var/atlassian/jira
ENV JIRA_INSTALL  /opt/atlassian/jira
ENV JIRA_VERSION  7.12.1
# 定义临时文件夹，用于存放上传的文件
ENV TEMP_PATH     /temp/jira

# 准备JIRA的安装工作
# 1. 创建/temp/jira临时目录
# 2. 将需要的文件上传到临时文件，有破解文件和JIRA的安装包等等
# 3. 将sources.list.163文件上传，用于替换成163源
# To Ready JIRA
RUN mkdir /temp && mkdir /temp/jira
COPY mysql-connector-java-5.1.42-bin.jar         /temp/jira/mysql-connector-java-5.1.42-bin.jar
COPY postgresql-42.2.1.jar                    /temp/jira/postgresql-42.2.1.jar
COPY atlassian-extras-3.2.jar                    /temp/jira/atlassian-extras-3.2.jar
COPY atlassian-jira-software-${JIRA_VERSION}.tar.gz        /temp/jira/atlassian-jira-software-${JIRA_VERSION}.tar.gz
COPY 163.com.sources.list                           /temp/jira/163.com.sources.list

# 安装JIRA的运行命令，主要做了以下工作：
# 1. 将debain的原始源替换成国内的163源
# 2. 将下载获取JIRA更换为上传JIRA的源码包，因为下载JIRA的速度实在有点感人
# Install Atlassian JIRA and helper tools and setup initial home
# directory structure.
RUN set -x \
    && mv                      /etc/apt/sources.list /etc/apt/sources.list.back \
    && cp                      "${TEMP_PATH}/163.com.sources.list" /etc/apt/sources.list \
    # && echo "deb http://mirrors.163.com/debian jessie-backports main" > /etc/apt/sources.list.d/jessie-backports.list \
    && apt-get update --quiet \
    && apt-get install --quiet --yes --no-install-recommends xmlstarlet \
    && apt-get install --quiet --yes --no-install-recommends -t jessie-backports libtcnative-1 \
    && apt-get clean \
    && mkdir -p                "${JIRA_HOME}" \
    && mkdir -p                "${JIRA_HOME}/caches/indexes" \
    && chmod -R 700            "${JIRA_HOME}" \
    && chown -R daemon:daemon  "${JIRA_HOME}" \
    && mkdir -p                "${JIRA_INSTALL}/conf/Catalina" \
    && cp                      "${TEMP_PATH}/atlassian-jira-software-${JIRA_VERSION}.tar.gz" "./atlassian-jira-software-${JIRA_VERSION}.tar.gz" \
    && tar -zxvf               "./atlassian-jira-software-${JIRA_VERSION}.tar.gz" --directory "${JIRA_INSTALL}" --strip-components=1 --no-same-owner \
    && cp                      "${TEMP_PATH}/mysql-connector-java-5.1.42-bin.jar" "${JIRA_INSTALL}/lib/mysql-connector-java-5.1.42-bin.jar" \
    && rm -f                   "${JIRA_INSTALL}/lib/postgresql-9.1-903.jdbc4-atlassian-hosted.jar" \
    && cp                      "${TEMP_PATH}/postgresql-42.2.1.jar" "${JIRA_INSTALL}/lib/postgresql-42.2.1.jar" \
    && chmod -R 700            "${JIRA_INSTALL}/conf" \
    && chmod -R 700            "${JIRA_INSTALL}/logs" \
    && chmod -R 700            "${JIRA_INSTALL}/temp" \
    && chmod -R 700            "${JIRA_INSTALL}/work" \
    && chown -R daemon:daemon  "${JIRA_INSTALL}/conf" \
    && chown -R daemon:daemon  "${JIRA_INSTALL}/logs" \
    && chown -R daemon:daemon  "${JIRA_INSTALL}/temp" \
    && chown -R daemon:daemon  "${JIRA_INSTALL}/work" \
    && sed --in-place          "s/java version/openjdk version/g" "${JIRA_INSTALL}/bin/check-java.sh" \
    && echo -e                 "\njira.home=$JIRA_HOME" >> "${JIRA_INSTALL}/atlassian-jira/WEB-INF/classes/jira-application.properties" \
    && touch -d "@0"           "${JIRA_INSTALL}/conf/server.xml"

# 使用补丁包替换掉原来的JAR包
# Hack
RUN set -x \
    && rm -rf                  "${JIRA_INSTALL}/atlassian-jira/WEB-INF/lib/atlassian-extras-3.2.jar" \
    && cp                      "${TEMP_PATH}/atlassian-extras-3.2.jar" "${JIRA_INSTALL}/atlassian-jira/WEB-INF/lib/atlassian-extras-3.2.jar"

# Use the default unprivileged account. This could be considered bad practice
# on systems where multiple processes end up being executed by 'daemon' but
# here we only ever run one process anyway.
USER daemon:daemon

# Expose default HTTP connector port.
EXPOSE 8080

# Set volume mount points for installation and home directory. Changes to the
# home directory needs to be persisted as well as parts of the installation
# directory due to eg. logs.
VOLUME ["/var/atlassian/jira", "/opt/atlassian/jira/logs"]

# Set the default working directory as the installation directory.
WORKDIR /var/atlassian/jira

COPY "docker-entrypoint.sh" "/"
ENTRYPOINT ["/docker-entrypoint.sh"]

# Run Atlassian JIRA as a foreground process by default.
CMD ["/opt/atlassian/jira/bin/start-jira.sh", "-fg"]
```

```shell
docker build -t atlassian/jira:7.12.1 .
```

```shell
chown -R 2:2 /data/docker/jira
```

```shell
docker run -p 8088:8080 -v /data/docker/jira:/var/atlassian/jira -d --name jira-7.12.1 atlassian/jira:7.12.1
```



**破解jira**

先关闭jira，然后把破解包里面的atlassian-extras-3.2.jar和mysql-connector-java- 5.1.42-bin.jar两个文件复制到/usr/local/atlassian/jira/atlassian-jira/WEB-INF/lib/目录下。

其中atlassian-extras-3.2.jar是用来替换原来的atlassian-extras-3.1.2.jar文件，用作破解jira系统的。

而mysql-connector-java- 5.1.42-bin.jar是用来连接mysql数据库的驱动软件包。

```shell
cd /opt/atlassian/jira/atlassian-jira/WEB-INF/lib/
```

**JIRA Software (Server): Evaluation**

```shell
AAABdQ0ODAoPeNp9kU9vgkAQxe98CpJe2sMSIG2tJiS1sDU0CEZsbZpetjjqNrKQ2QXrty8CjVr/H
Iflzfu9N1dTmOkx5Lp5p9tWz+72TFN3vYlum9aDtkAAsczyHNAIeAJCAp1xxTPh0HBCx6OxH1MtL
NIvwGj+KgGlQyzNzYRiiQpZCk5WrpmAx0XK+MpIslT75siMI8mowGTJJHhMgbP1JmaXmPda6zrZ5
FCvc6PhkI5dvx/8PdGfnONmp7PMra5FoMPK9pghBiwBfc95eh6MyAcdDEg4jfskCN+jBjDHbFYky
tgORGZztWYIRrWRl+AoLKD57XwpJ6o7FaLiEwoEE8mZIBdojkpsfapcge/FNCSBZdudjt251arJO
fxyYXGsGCpAZ85WErQIF0xwyeqER1W6CPXL/8OtGpa3Cm2rsw8KgSoz5shl26UHMkGe1w4v/rivx
y2Kft2c6uazp9OSrYraq2E/d4xTNe+b7+t2O5v5F6LlDH4wLAIUCfrbZmVmXsLs8QU/ckLP/eLCD
JsCFCRRqQYfO2gW3mlydfYyj5FIcjiYX02i6
```

**JIRA Core (Server): Evaluation**

```shell
AAABVQ0ODAoPeNp1kdtOg0AQhu95ChJv9GIbDtYekk2ssIkYSitUmxhv1u1U18BClgXt27tAm1axl
3P65/tnLtawMRMoTGtoOtbUcafuyPT8lQ7ssRFV2RvIxfapBFliZBteLhRlKqIZ4Lz+ogJu3zPK0
wHLM+OTSzooZL6pmBo0AWK5hEFPpO3rZZeVZB+0BJ8qwM12ZE2QdWOEnIEoYbUroF3rLeZzEnvBL
DyUyHfB5e44Z1vN3B6VzDVenzUBWYMMfHx3P3tEy3A4RC8TN0HX8To+ZyRRVCqQeEvTErqmA8GGK
54LTKIViZdxkJBzGpqI14CVrMDQZEKBoIKdsbBX15Rh4CckQqHtOKORNXYNHeHfmYV8p4KXtAXp+
fUktJW/1027Fc/6Cc2cY/hQMsmLVuUhiGemp7HNy+5eV69Tk9Q0rVqtzuOpo9Ob/Pew42zX/wNzj
NUrMCwCFEEnmeijjMtt7NE0TXO1VXvHnosCAhRApci+OpA4MTtE0d/t7LAIH4gmkw==X02gs
```



## 参考

[[烂泥：jira7.3/7.2安装、中文及破解(20170829更新)](https://www.ilanni.com/?p=12119)](https://www.ilanni.com/?p=12119)

[CentOS_7_Jira7.4.1安装部署_及破解](http://www.llbee.com/?p=609)

[Docker JIRA 7.3.8 破解部署](https://xuqiang.me/Docker-JIRA-7-3-8-%E7%A0%B4%E8%A7%A3%E9%83%A8%E7%BD%B2.html)

[Docker+centos7+jira7.3.6+postgre Sql破解搭建](http://xumf.net/blog/docker+centos7+jira7.3/)

[docker入门到实战(5)安装mysql容器](https://www.jianshu.com/p/b48eb3297dee)