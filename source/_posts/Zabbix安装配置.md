title: Zabbix安装配置
date: 2016-12-18 21:07:00
categories:
- 技术
- Linux
tags:
- CentOS
- 监控
- Zabbix
---
Zabbix安装配置
>Zabbix 3.0
>Zabbix 3.4

[Zabbix手册](https://www.zabbix.com/documentation/3.4/zh/manual)[Zabbix安装要求](https://www.zabbix.com/documentation/3.4/zh/manual/installation/requirements)

# 服务端
## 安装源码库配置部署包
安装源码库配置部署包。这个部署包包含了yum配置文件。
查询最新的包访问[Zabbix repo](http://repo.zabbix.com/zabbix/3.4/rhel/7/x86_64/)

```
rpm -ivh http://repo.zabbix.com/zabbix/3.4/rhel/7/x86_64/zabbix-release-3.4-2.el7.noarch.rpm
```

## 安装Zabbix部署包
安装Zabbix部署包。以下是使用Mysql数据库安装Zabbix server、WEB前端

```
yum install -y zabbix-server-mysql zabbix-web-mysql
```

# 安装初始化数据库

在MySQL上安装Zabbix数据库和用户，请参看下列指导步骤。

MySQL数据库创建脚本。

```
mysql -uroot -p

create database zabbix character set utf8 collate utf8_bin;
grant all privileges on zabbix.* to zabbix@localhost identified by 'zabbix';
exit;
```

然后导入初始架构（Schema）和数据。

```
cd /usr/share/doc/zabbix-server-mysql-3.4.4
zcat create.sql.gz | mysql -uroot -p zabbix
```

# 启动Zabbix Server进程
在zabbix_server.conf中编辑数据库配置

```
vi /etc/zabbix/zabbix_server.conf

DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=zabbix
```

启动Zabbix Server进程

```
systemctl start zabbix-server
```

# 编辑Zabbix前端的PHP配置
修改Zabbix前端的Apache配置文件位于 。
/etc/httpd/conf.d/zabbix.conf 

```
php_value max_execution_time 300
php_value memory_limit 128M
php_value post_max_size 16M
php_value upload_max_filesize 2M
php_value max_input_time 300
php_value always_populate_raw_post_data -1
# php_value date.timezone Europe/Riga
```
据所在时区，你可以取消 “date.timezone” 设置的注释，并正确配置它。

Zabbix前端可以在浏览器中通过 http://IP/zabbix 进行访问。默认的用户名／密码为 Admin/zabbix

初始化配置后，最终会生成如下文件：/etc/zabbix/web/zabbix.conf.php


# 客户端
## 安装源码库配置部署包
安装源码库配置部署包。这个部署包包含了yum配置文件。

```
rpm -ivh http://repo.zabbix.com/zabbix/3.4/rhel/7/x86_64/zabbix-release-3.4-2.el7.noarch.rpm
```

## 安装Zabbix Agent
```
yum install zabbix-agent
```

## 配置
```
vi /etc/zabbix/zabbix_agentd.conf
Server=10.8.8.8
```

## 启动
```
systemctl start zabbix-agent
systemctl enable zabbix-agent
systemctl status zabbix-agent
```

