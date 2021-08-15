---
title: MariaDB使用笔记
date: 2017-06-24 08:23:05
---

# MariaDB



<!--more-->



## 部署

### Docker 方式

 [mariadb - Docker Hub](https://hub.docker.com/_/mariadb) 

```
docker pull mariadb:10.5.2
```

```shell
docker run -d --name mariadb -p 3306:3306 -v ~/docker/mariadb:/var/lib/mysql -e TIMEZONE=Asia/Shanghai -e MYSQL_ROOT_PASSWORD=root -e SERVER_ID=1 mariadb:10.5.2
```

初始化一个脚本（支持`.sh, .sql, .sql.gz`），添加文件到 `/docker-entrypoint-initdb.d`。

s

 [mysql - Docker Hub](https://hub.docker.com/_/mysql) 

```bash
docker run -d --name mysql -p 3306:3306 -v ~/docker/mysql:/var/lib/mysql -e TIMEZONE=Asia/Shanghai -e MYSQL_ROOT_PASSWORD=root -e SERVER_ID=1 mysql:5.7.29
```



### macOS

```bash
brew tap homebrew/services
```

安装mariadb
```bash
brew install mariadb
```

启动mariadb
```bash
brew services start mariadb
```

查看服务列表
```bash
brew services list
```



### CentOS

```
rpm --import http://yum.mariadb.org/RPM-GPG-KEY-MariaDB
```

```
cat >/etc/yum.repos.d/mariadb.repo<<EOF
# MariaDB 10.2 CentOS repository list - created 2018-04-20 08:34 UTC
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.2/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
EOF
```

```shell
yum update
yum list --showduplicates MariaDB-server
```

```shell
yum -y install MariaDB-server-10.2.13 MariaDB-client-10.2.13 MariaDB-common-10.2.13 MariaDB-compat-10.2.13 MariaDB-shared-10.2.13
```



编码配置

```
# 编辑/etc/my.cnf
vim /etc/my.cnf

# 在[mysqld]标签下添加下面内容
default_storage_engine = innodb
innodb_file_per_table
max_connections = 4096
collation-server = utf8_general_ci
character-set-server = utf8
```

```

# 编辑/etc/my.cnf.d/client.cnf
vim /etc/my.cnf.d/client.cnf

# 在[client]标签下添加下面内容
default-character-set=utf8
```

```
# 编辑/etc/my.cnf.d/mysql-clients.cnf
vim /etc/my.cnf.d/mysql-clients.cnf

# 在[mysql]标签下添加下面内容
default-character-set=utf8
```

启动

```shell
systemctl enable mariadb.service
systemctl start mariadb.service
systemctl status mariadb.service
systemctl stop mariadb.service
```

设置密码

```
mysql_secure_installation
```
> [Setting up MariaDB Repositories](https://downloads.mariadb.org/mariadb/repositories/#mirror=neusoft&distro=CentOS&distro_release=centos7-amd64--centos7&version=10.2)
>
>  [Installing MariaDB with yum](https://mariadb.com/kb/en/library/yum/) 
>
> [Centos 7 下 mariadb 的安装与配置](https://www.jianshu.com/p/e67f1dbaa107)   
>
> [设置字符集和排序规则](https://mariadb.com/kb/zh-cn/setting-character-sets-and-collations/)



## 设置

### 慢查询

> [MySQL慢查询（一） - 开启慢查询 - 成九 - 博客园](https://www.cnblogs.com/luyucheng/p/6265594.html) 

查看

```sql
show variables like 'slow_query%';
```



时间

```sql
show variables like 'long_query_time';
```



查看慢日志条数

```sql
show global status like '%slow_queries%'
```



设置方法一，SQL语句

将 slow_query_log 全局变量设置为“ON”状态

```sql
 set global slow_query_log='ON'; 
```

设置慢查询日志存放的位置

```sql
set global slow_query_log_file='/tmp/slow.log';
```

查询超过1秒就记录

```sql
set global long_query_time=1;
```



设置方法二，my.cnf 配置文件。更改文件后，重启服务。

```
[mysqld]
slow_query_log = ON
slow_query_log_file = /tmp/slow.log
long_query_time = 1
```



### 全局查询日志

> [MySQL分析SQL耗时瓶颈_charming的专栏-CSDN博客](https://blog.csdn.net/zxc123e/article/details/77908432) 



### show profile

> show profile命令可以分析当前会话中语句执行的资源消耗情况。用于查找SQL耗时瓶颈。



## [MariaDB 教程](https://mariadb.com/kb/zh-cn/configuring-mariadb-for-remote-client-access/)

基础知识

##### 列出数据库

show databases;

##### 新建数据库

```
create database proxy_ip;
```

##### 新建数据表

```
create table xici_proxy(ip varchar(20) COMMENT '地址',
port varchar(5) COMMENT '端口', 
proxy_type varchar(5) COMMENT '类型', 
speed varchar(10) NULL COMMENT '速度',
PRIMARY KEY(ip)
)ENGINE=InnoDB DEFAULT CHARSET=utf8 COMMENT='西刺代理IP池';
```

##### 查看表结构

```
desc xici_proxy;
```

##### 查看表生成的DDL

```
show create table xici_proxy;
```

##### 删除数据库

```shell
drop database xici_proxy;
```

##### 查看字符集

```shell
show variables like 'collation_%';
```

##### 查看字符集

```shell
show variables like 'character_set_%';
```



用户授权

```mysql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mypwd' WITH GRANT OPTION;
flush privileges;
```



#### [修改密码](https://www.cnblogs.com/liufei88866/p/5619215.html)
方法1： 用SET PASSWORD命令

```sql
SET PASSWORD FOR 'root'@'localhost' = PASSWORD('newpass');
```


方法2：用mysqladmin

```sql
mysqladmin -u root password "newpass"

如果root已经设置过密码，采用如下方法

mysqladmin -u root password oldpass "newpass"
```


方法3： 用UPDATE直接编辑user表

```sql
mysql -u root

mysql> use mysql;

mysql> UPDATE user SET Password = PASSWORD('newpass') WHERE user = 'root';

mysql> FLUSH PRIVILEGES;
```



在丢失root密码的时候，可以这样

```sql
mysqld_safe --skip-grant-tables&

mysql -u root mysql

mysql> UPDATE user SET password=PASSWORD("new password") WHERE user='root';

mysql> FLUSH PRIVILEGES;
```



#### 添加用户，设置权限

- 创建用户（本地无法远程登录）

```mysql
create user <username>@localhost identified by '<password>';
```

- 直接创建用户并授权的命令（本地无法远程登录）

```mysql
grant all on *.* to <username>@localhost identified by '<password>';
```

- 授予远程登陆权限

```mysql
grant all privileges on *.* to <username>@'%' identified by '<password>';
```

- 授予权限并且可以授权（指定 hostname 操作）

```mysql
grant all privileges on *.* to <username>@'<hostname>' identified by '<password>' with grant option;
```



#### 查看版本

```
mysqladmin -u root -p version
```



查看字符集

```mysql
show variables like "%character%";show variables like "%collation%";

#默认的字符集
show global variables like '%char%';

#当前数据库支持的字符集
show character set;
```



#### 数据库导出导入

导入数据库

```mysql
source future.sql;
```



### mysql表名忽略大小写

```sql
show variables like "%case%";
```

 [mysql表名忽略大小写_wocjj的专栏-CSDN博客_mysql 忽略大小写](https://blog.csdn.net/wocjj/article/details/7415200) 




## 参考

[configuring-mariadb-for-remote-client-access](https://mariadb.com/kb/zh-cn/configuring-mariadb-for-remote-client-access/)

