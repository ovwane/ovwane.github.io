---
title: MariaDB使用笔记
date: 2017-06-24 08:23:05
---
MariaDB 10.2.13

### [修改密码](https://www.cnblogs.com/liufei88866/p/5619215.html)
#### 方法1： 用SET PASSWORD命令
```
mysql -u root -p

mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('newpass');
```
```
方法2：用mysqladmin

　　mysqladmin -u root password "newpass"

　　如果root已经设置过密码，采用如下方法

　　mysqladmin -u root password oldpass "newpass"

方法3： 用UPDATE直接编辑user表

　　mysql -u root

　　mysql> use mysql;

　　mysql> UPDATE user SET Password = PASSWORD('newpass') WHERE user = 'root';

　　mysql> FLUSH PRIVILEGES;

在丢失root密码的时候，可以这样

　　mysqld_safe --skip-grant-tables&

　　mysql -u root mysql

　　mysql> UPDATE user SET password=PASSWORD("new password") WHERE user='root';

　　mysql> FLUSH PRIVILEGES;
```

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

## 参考

[configuring-mariadb-for-remote-client-access](https://mariadb.com/kb/zh-cn/configuring-mariadb-for-remote-client-access/)