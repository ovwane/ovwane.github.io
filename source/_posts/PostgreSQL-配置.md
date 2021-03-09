---
title: PostgreSQL 配置
date: 2020-04-16 14:52:12
tags:
---

<!--more-->

```
psql -U superset -W superset -h 127.0.0.1 -p 5432 -d superset
```



列出所有数据库

```bash
\l
```



备份数据库中的表数据

```shell
pg_dump -U superset -Fc -t ab_user -f ab_user.sql superset
```



恢复数据库中的表数据

```shell
psql superset;
DROP TABLE ab_user;

pg_restore -U superset -a -t ab_user -d superset ab_user.sql
```



导出表数据

```shell
COPY (select * from ab_user where id>1) TO '/tmp/user.csv' WITH csv;
```

>选择用户 id 大于 1的。



导入表数据

```shell
COPY ab_user FROM '/tmp/user.csv' WITH csv;
```



## 参考

 [Postgres copy命令导入导出数据_wtopps的专栏-CSDN博客](https://blog.csdn.net/wtopps/article/details/79097748) 