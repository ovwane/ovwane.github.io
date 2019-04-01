---
title: Confluence 配置
date: 2019-04-01 17:47:51
tags:
---



### MySQL 数据库默认排序

> [How to Fix the Collation and Character Set of a MySQL Database](<https://confluence.atlassian.com/kb/how-to-fix-the-collation-and-character-set-of-a-mysql-database-744326173.html>)



### Confluence 中文乱码

> [解决confluence的乱码问题](<https://blog.csdn.net/lovely_xinyi/article/details/45581883>)

```mysql
show variables like 'char%';
```

> 修改mysql安装文件夹下的my.ini文件可以解决该问题
>
> [mysqld]
>
> collation_server=utf8_unicode_ci
>
> character_set_server=utf8