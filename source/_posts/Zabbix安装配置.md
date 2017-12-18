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

[Zabbix](https://www.zabbix.com/)
>CentOS 7.4.1708
>Zabbix 
# Zabbix安装配置
主机

|IP|用途|角色|版本|软件版本|
|:|
|10.8.8.81|服务器|Zabbix Server|zabbix-server zabbix-web
|10.8.8.82|数据库|MariaDB|
|10.8.8.83|代理服务器|Zabbix Proxy Server|

```
systemctl stop firewalld;systemctl disable firewalld
setenforce 0;sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

## 安装Zabbix
### rpm安装
## 下载Zabbix安装包
[Zabbix Download](https://www.zabbix.com/download)

#### 添加zabbix yum源
```
rpm -ivh http://repo.zabbix.com/zabbix/3.4/rhel/7/x86_64/zabbix-release-3.4-2.el7.noarch.rpm
```

#### 安装服务端
```
yum install zabbix-server-mysql
yum install zabbix-web-mysql
```
```
Installing:
 zabbix-server-mysql   x86_64   3.4.4-2.el7        zabbix                 2.0 M
Installing for dependencies:
 OpenIPMI-libs         x86_64   2.0.19-15.el7      base                   502 k
 OpenIPMI-modalias     x86_64   2.0.19-15.el7      base                    15 k
 fping                 x86_64   3.10-1.el7         zabbix-non-supported    40 k
 gnutls                x86_64   3.3.26-9.el7       base                   677 k
 iksemel               x86_64   1.4-2.el7.centos   zabbix-non-supported    49 k
 libevent              x86_64   2.0.21-4.el7       base                   214 k
 libtool-ltdl          x86_64   2.4.2-22.el7_3     base                    49 k
 net-snmp-libs         x86_64   1:5.7.2-28.el7     base                   748 k
 nettle                x86_64   2.7.1-8.el7        base                   327 k
 trousers              x86_64   0.3.14-2.el7       base                   289 k
 unixODBC              x86_64   2.3.1-11.el7       base                   413 k
```
```
Installing:
 zabbix-web-mysql           noarch    3.4.4-2.el7              zabbix     6.4 k
Installing for dependencies:
 apr                        x86_64    1.4.8-3.el7              base       103 k
 apr-util                   x86_64    1.5.2-6.el7              base        92 k
 dejavu-fonts-common        noarch    2.33-6.el7               base        64 k
 dejavu-sans-fonts          noarch    2.33-6.el7               base       1.4 M
 fontpackages-filesystem    noarch    1.44-8.el7               base       9.9 k
 httpd                      x86_64    2.4.6-67.el7.centos.6    updates    2.7 M
 httpd-tools                x86_64    2.4.6-67.el7.centos.6    updates     88 k
 libX11                     x86_64    1.6.5-1.el7              base       606 k
 libX11-common              noarch    1.6.5-1.el7              base       164 k
 libXau                     x86_64    1.0.8-2.1.el7            base        29 k
 libXpm                     x86_64    3.5.12-1.el7             base        55 k
 libjpeg-turbo              x86_64    1.2.90-5.el7             base       134 k
 libpng                     x86_64    2:1.5.13-7.el7_2         base       213 k
 libxcb                     x86_64    1.12-1.el7               base       211 k
 libxslt                    x86_64    1.1.28-5.el7             base       242 k
 libzip                     x86_64    0.10.1-8.el7             base        48 k
 mailcap                    noarch    2.1.41-2.el7             base        31 k
 php                        x86_64    5.4.16-43.el7_4          updates    1.4 M
 php-bcmath                 x86_64    5.4.16-43.el7_4          updates     57 k
 php-cli                    x86_64    5.4.16-43.el7_4          updates    2.7 M
 php-common                 x86_64    5.4.16-43.el7_4          updates    565 k
 php-gd                     x86_64    5.4.16-43.el7_4          updates    127 k
 php-ldap                   x86_64    5.4.16-43.el7_4          updates     52 k
 php-mbstring               x86_64    5.4.16-43.el7_4          updates    505 k
 php-mysql                  x86_64    5.4.16-43.el7_4          updates    101 k
 php-pdo                    x86_64    5.4.16-43.el7_4          updates     98 k
 php-xml                    x86_64    5.4.16-43.el7_4          updates    125 k
 t1lib                      x86_64    5.1.2-14.el7             base       166 k
 zabbix-web                 noarch    3.4.4-2.el7              zabbix     2.5 M
```

配置Zabbix数据库

```
# vi /etc/zabbix/zabbix_server.conf
DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=<password>
```

设置php时区

```
vi /etc/httpd/conf.d/zabbix.conf

# php_value date.timezone Europe/Riga
```

SELinux configuration

```
Having SELinux status enabled in enforcing mode, you need to execute the following commands to enable communication between Zabbix frontend and server:

RHEL 7 and later:

# setsebool -P httpd_can_connect_zabbix on
If the database is accessible over network (including 'localhost' in case of PostgreSQL), you need to allow Zabbix frontend to connect to the database too:
# setsebool -P httpd_can_network_connect_db on
RHEL prior to 7:

# setsebool -P httpd_can_network_connect on
# setsebool -P zabbix_can_network on
As frontend and SELinux configuration is done, you need to restart Apache web server:
# service httpd restart
```

```
systemctl enable zabbix-server
systemctl start zabbix-server
systemctl enable httpd
systemctl restart httpd
```

#### 安装数据库服务器
```
yum install mariadb-server mariadb
```
```
Installing:
 mariadb-server               x86_64      1:5.5.56-2.el7        base       11 M
Installing for dependencies:
 perl-Compress-Raw-Bzip2      x86_64      2.061-3.el7           base       32 k
 perl-Compress-Raw-Zlib       x86_64      1:2.061-4.el7         base       57 k
 perl-DBD-MySQL               x86_64      4.023-5.el7           base      140 k
 perl-DBI                     x86_64      1.627-4.el7           base      802 k
 perl-Data-Dumper             x86_64      2.145-3.el7           base       47 k
 perl-IO-Compress             noarch      2.061-2.el7           base      260 k
 perl-Net-Daemon              noarch      0.48-5.el7            base       51 k
 perl-PlRPC                   noarch      0.2020-14.el7         base       36 k
```
```
Installing:
 mariadb                     x86_64      1:5.5.56-2.el7         base      8.7 M
Installing for dependencies:
 perl                        x86_64      4:5.16.3-292.el7       base      8.0 M
 perl-Carp                   noarch      1.26-244.el7           base       19 k
 perl-Encode                 x86_64      2.51-7.el7             base      1.5 M
 perl-Exporter               noarch      5.68-3.el7             base       28 k
 perl-File-Path              noarch      2.09-2.el7             base       26 k
 perl-File-Temp              noarch      0.23.01-3.el7          base       56 k
 perl-Filter                 x86_64      1.49-3.el7             base       76 k
 perl-Getopt-Long            noarch      2.40-2.el7             base       56 k
 perl-HTTP-Tiny              noarch      0.033-3.el7            base       38 k
 perl-PathTools              x86_64      3.40-5.el7             base       82 k
 perl-Pod-Escapes            noarch      1:1.04-292.el7         base       51 k
 perl-Pod-Perldoc            noarch      3.20-4.el7             base       87 k
 perl-Pod-Simple             noarch      1:3.28-4.el7           base      216 k
 perl-Pod-Usage              noarch      1.63-3.el7             base       27 k
 perl-Scalar-List-Utils      x86_64      1.27-248.el7           base       36 k
 perl-Socket                 x86_64      2.010-4.el7            base       49 k
 perl-Storable               x86_64      2.45-3.el7             base       77 k
 perl-Text-ParseWords        noarch      3.29-4.el7             base       14 k
 perl-Time-HiRes             x86_64      4:1.9725-3.el7         base       45 k
 perl-Time-Local             noarch      1.2300-2.el7           base       24 k
 perl-constant               noarch      1.27-2.el7             base       19 k
 perl-libs                   x86_64      4:5.16.3-292.el7       base      688 k
 perl-macros                 x86_64      4:5.16.3-292.el7       base       43 k
 perl-parent                 noarch      1:0.225-244.el7        base       12 k
 perl-podlators              noarch      2.5.1-3.el7            base      112 k
 perl-threads                x86_64      1.87-4.el7             base       49 k
 perl-threads-shared         x86_64      1.43-6.el7             base       39 k
```
```
systemctl enable mariadb
systemctl start mariadb
```
```
mysql_secure_installation
```

##### 新建数据库
```
mysql -uroot -p

create database zabbix character set utf8 collate utf8_bin;
#和zabbix server安装在一个服务器
grant all privileges on zabbix.* to zabbix@localhost identified by 'zabbix';
#独立服务器，允许10.8.8.0网段的IP访问。
grant all privileges on zabbix.* to zabbix@'10.8.8.%' identified by 'zabbix';
quit;
```

##### 倒入数据库模版
create.sql.gz这个文件由zabbix-server-mysql软件包提供，数据库是单独安装的。需要从zabbix-server上拷贝到数据库服务器。

```
scp root@10.8.8.81:/usr/share/doc/zabbix-server-mysql-3.4.4/create.sql.gz /root/create.sql.gz

zcat /root/create.sql.gz | mysql -uzabbix -p zabbix
```
```
zcat /usr/share/doc/zabbix-server-mysql/create.sql.gz | mysql -uzabbix -p zabbix
```

登录 
http://IP/zabbix
用户名：Admin
密码：zabbix

#### 安装代理服务器
```
rpm -ivh http://repo.zabbix.com/zabbix/3.4/rhel/7/x86_64/zabbix-release-3.4-2.el7.noarch.rpm
```
```
yum -y install zabbix-proxy-mysql
```
```
zcat /usr/share/doc/zabbix-proxy-mysql/schema.sql.gz | mysql -uzabbix -p zabbix
```

Configure database for Zabbix server/proxy

```
# vi /etc/zabbix/zabbix_server.conf
DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=<password>
```

#### 安装agent客户端
```
rpm -ivh http://repo.zabbix.com/zabbix/3.4/rhel/7/x86_64/zabbix-release-3.4-2.el7.noarch.rpm
```
```
yum -y install zabbix-agent
```
```
Installing:
 zabbix-agent         x86_64         3.4.4-2.el7           zabbix         358 k
```

```
```
/etc/zabbix/zabbix_agentd.conf
Server=127.0.0.1
ServerActive=127.0.0.1
Hostname=Zabbix server
```
systemctl enable zabbix-agent
systemctl start zabbix-agent
systemctl status zabbix-agent
```

### 源码安装
## 配置Zabbix
