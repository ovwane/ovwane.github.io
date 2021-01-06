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



## MariaDB 数据类型

- 字符类型

| 类型       | 说明                          | 范围                                |
| ---------- | ----------------------------- | ----------------------------------- |
| CHAR       | 固定长度的非 Unicode 字符数据 | 0~255 长度                          |
| VARCHAR    | 可变长度的非 Unicode 数据     | 0~65535 长度                        |
| BINARY     | 二进制字节字符串              |                                     |
| VARBINARY  | 可变长度的二进制字节字符串    |                                     |
| BLOB       | blob 列                       | 0~65535 长度                        |
| TINYBLOB   | blob 列                       | 0~255 长度                          |
| MEDIUMBLOB | blob 列                       | 0-2^24 字节，16,777,215 最大长度    |
| LONGBLOB   | blob 列                       | 0-2^32 字节，4,294,967,295 最大长度 |
| TEXT       | 字符的文本列                  | 0~65635 长度                        |
| TINYTEXT   | 字符的文本列                  | 0~255 长度                          |
| MEDIUMTEXT | 字符的文本列                  | 0-2^24 字节，16,777,215 最大长度    |
| LONGTEXT   | 字符的文本列                  | 0-2^32 字节，4,294,967,295 最大长度 |

- 使用修饰符

| 修饰符        | 说明                           |
| ------------- | ------------------------------ |
| NULL          | 可以为空值                     |
| NOT NULL      | 不能为空值                     |
| DEFAULT #     | 设置默认值，不适用于 text 类型 |
| character set | 修改字符集                     |

- 使用通配符

| 通配符 | 说明                   |
| ------ | ---------------------- |
| %      | 匹配任意长度的任意字符 |
| _      | 匹配任意单个字符       |



### 数值类型

| 类型         | 说明   | 范围                                                         |
| ------------ | ------ | ------------------------------------------------------------ |
| INT          | 整数   | 0-2^32 字节，0~4,294,967,295                                 |
| TINYINT      | 整数   | -128~127 的有符号范围，0~255 的无符号范围                    |
| SMALLINT     | 整数   | -32768~32768 的有符号范围，0~65535 的无符号范围              |
| MEDIUMINT    | 整数   | -8388608~8388607 的有符号范围，0~16777215 的无符号范围       |
| BIGINT       | 整数   | 有符号范围 -9223372036854775808~9223372036854775807内整数，无符号范围 0 ~18446744073709551615 的整数 |
| BOOLEAN      | 布尔值 | 值 0 与 false 相关联，值 1 与 true 相关联                    |
| DECIMAL(m,d) | 小数   | m 表示数字的总长度，小数点不占位，d 表示小数点后面的数字长度。 |
| FLOAT        | 小数   | -3.402823466E+38~-1.175494351E-38，1.175494351E-38~3.402823466E+38 |
| DOUBLE       | 小数   | -1.7976931348623157E+308~-2.2250738585072014E-308，2.2250738585072014E-308~1.7976931348623157E+308 |

- 使用修饰符

| 修饰符         | 说明         |
| -------------- | ------------ |
| UNSIGNED       | 无符号数值   |
| NULL           | 可以为空值   |
| NOT NULL       | 不能为空值   |
| DEFAULT #      | 设置默认值   |
| AUTO_INCREMENT | 数值自动增长 |

- 日期时间类型

| 类型      | 说明     | 范围                                                         |
| --------- | -------- | ------------------------------------------------------------ |
| DATE      | 日期     | 1000-01-01 到 9999-12-31，并使用 yyyy-MM-dd 日期格式         |
| TIME      | 时间     | -838：59：59.999999 到 838：59：59.999999 时间范围           |
| DATETIME  | 日期时间 | 1000-01-01 00：00：00.000000 至 9999-12-31 23：59：59.999999。使用 yyyy-MM-dd hh：mm：ss 格式 |
| TIMESTAMP | 时间戳   | 19700101080001-20380119111407                                |
| YEAR      | 年份     | 1901 到 2155 和 0000 范围内的值                              |

- 使用修饰符

| 修饰符    | 说明       |
| --------- | ---------- |
| NULL      | 可以为空值 |
| NOT NULL  | 不能为空值 |
| DEFAULT # | 设置默认值 |

- 如何设置字段自动获取当前时间
  - 将字段类型设为  **TIMESTAMP**
  - 将默认值设为 **CURRENT_TIMESTAMP**

### 内建类型

| 类型 | 说明           | 范围                                       |
| ---- | -------------- | ------------------------------------------ |
| ENUM | 枚举类型       | 仅能从给出的选项中选择其中一个，其它值无效 |
| SET  | 集合类型       | 使用给出的元素组合成字符串                 |
| JSON | 原生 JSON 对象 |                                            |

- 使用修饰符

| 修饰符    | 说明       |
| --------- | ---------- |
| NULL      | 可以为空值 |
| NOT NULL  | 不能为空值 |
| DEFAULT # | 设置默认值 |

- JSON 使用方法
  - JSON_EXTRACT(context,'$.<key>') 查询提取 JSON 对象的某个属性值。
  - JSON_KEYS(context) 查询获取健对。
  - JSON_INSERT(context,'$.<key>',"<value>") 插入 JSON 属性与值。
  - JSON_SET(context,'$.<key>',"<value>") 更改 JSON 属性的值。
  - JSON_REMOVE(context,'$.<key>') 删除 JSON 属性。

参考



### mysql表名忽略大小写

```sql
show variables like "%case%";
```

 [mysql表名忽略大小写_wocjj的专栏-CSDN博客_mysql 忽略大小写](https://blog.csdn.net/wocjj/article/details/7415200) 




## 参考

[configuring-mariadb-for-remote-client-access](https://mariadb.com/kb/zh-cn/configuring-mariadb-for-remote-client-access/)

