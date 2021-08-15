---
title: mysqlreport 使用
date: 2021-08-15 16:23:25
tags:
---

# [mysqlreport](https://hackmysql.com/archive/mysqlreport/)



<!--more-->

安装

```
yum -y install perl-DBI perl-DBD-MySQL

yum -y install mysqlreport
```



使用

```
mysqlreport --host 172.21.0.2 --port 3306 --user root --password secret --no-mycnf --relative 10  --outfile=mysqlreport.txt --report-count 1 

mysqlreport --host 172.21.0.2 --port 3306 --user redmine --password secret --no-mycnf --relative 10  --outfile=mysqlreport.txt --report-count 1 
```



