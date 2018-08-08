---
title: Cacti安装配置
date: 2017-01-10 17:50:18
categories:
- 技术
- Linux
tags:
- CentOS
- 监控
- Cacti
---

Nagios安装配置

[下载Cacti](http://www.cacti.net/downloads)
[Linux安装Cacti](http://maocong.blog.51cto.com/4706737/1661680)

监控软件Cacti搭建
# 环境准备
## 安装epel扩展源
```
yum install -y epel-release
```

## 搭建lamp环境
```
[root@cacti ~]# yum install -y  httpd php php-mysql mysql mysql-server mysql-devel php-gd  libjpeg libjpeg-devel libpng-devel
```

```
/etc/init.d/httpd start
/etc/init.d/mysqld start
#初始化mysql
/usr/bin/mysqladmin -u root password 'new-password'
/usr/bin/mysqladmin -u root -h cacti password 'new-password'
#或者
/usr/bin/mysql_secure_installation
```

## 服务端
### 安装cacti监控主机
```
yum install -y cacti  net-snmp  net-snmp-utils  rrdtool net-snmp-devel net-snmp-libs lm-sensors php-xml zlib libpng freetype cairo-devel pango-devel gd
```
```
/etc/init.d/snmpd start
#新建cacti数据库
mysql -u root
mysql> create database cacti;
mysql> grant all on cacti.* to 'cactiuser'@'localhost' identified by 'cactiuser'; 
mysql> exit
#导入cacti数据库结构
mysql -u root cacti < /usr/share/doc/cacti-0.8.8b/cacti.sql
#设置cacti配置文件中的mysql数据库账号密码
vim /usr/share/cacti/include/config.php
$database_type = "mysql";
$database_default = "cacti";
$database_hostname = "localhost";
$database_username = "cactiuser";
$database_password = "cactiuser";
$database_port = "3306";
$database_ssl = false;
 
#修改httpd中的cacti配置
vim /etc/httpd/conf.d/cacti.conf
Deny from all 修改为 Allow from all
#重启httpd
/etc/init.d/httpd restart
```
通过浏览器访问
如果访问不了，请检查主机的iptables和selinux
到了登陆，默认的账号为admin，密码为admin
登陆之后，系统会强制用户更改密码
点击graphs-Default Tree-Host Localhost，进入系统监控界面
我们看到监控界面，没有出图，设置一下出图
```
/usr/bin/php /usr/share/cacti/poller.php 
OK u:0.00 s:0.00 r:0.00
OK u:0.00 s:0.00 r:0.01
OK u:0.00 s:0.00 r:0.03
OK u:0.00 s:0.00 r:0.05
OK u:0.00 s:0.00 r:0.08
06/13/2015 09:59:49 PM - SYSTEM STATS: Time:0.1974 Method:cmd.php Processes:1 Threads:N/A Hosts:2 HostsPerProcess:2 DataSources:5 RRDsProcessed:5

crontab -e
# 让命令每5分钟执行一次
*/5 * * * * /usr/bin/php /usr/share/cacti/poller.php > /dev/null 2>&1
```
我们在刷新一下，图就出来了, 数据需要等待一会儿，才能出来

接下来我们添加被监控的主机
## 客户端
在被监控的主机上安装

```
yum install -y net-snmp lm_sensors
```

vim /etc/snmp/snmpd.conf

 ```
#syslocation Unknown (edit /etc/snmp/snmpd.conf)
syslocation 192.168.1.118
 
#       group          context sec.model sec.level prefix read   write  notif
access  notConfigGroup ""      any       noauth    exact  all none none
 
view all    included  .1                               80    去掉注释符“#”
 ```

启动 snmpd

```
 /etc/init.d/snmpd start
```

## 使用
- 依次点击console -> Devices -> add,添加主机
- 开始添加被监控主机的信息，填写完毕，点击Create
- 创建完毕，看是否通信正常，创建完主机，创建要监控的项目点击 Create Graphs for this Host，
- 根据需要，去选择要监控的项目
- 监控项目添加完毕，将主机添加到监控主干线上，点击左侧Graph Trees
- 点击Add，添加被监控的主机
 - 类型选择Host，再选择要添加的主机
 - 添加完毕，点击Save
- 到主界面查看，是否添加成功，最好在监控服务器上刷新一下

```
/usr/bin/php /usr/share/cacti/poller.php --force
OK u:0.00 s:0.00 r:0.00
OK u:0.00 s:0.00 r:0.01
OK u:0.00 s:0.00 r:0.03
OK u:0.00 s:0.00 r:0.05
OK u:0.00 s:0.00 r:0.08
OK u:0.00 s:0.00 r:0.09
OK u:0.00 s:0.00 r:0.09
OK u:0.00 s:0.00 r:0.09
OK u:0.00 s:0.00 r:0.09
OK u:0.00 s:0.00 r:0.09
OK u:0.00 s:0.00 r:0.09
OK u:0.00 s:0.00 r:0.09
OK u:0.00 s:0.00 r:0.09
OK u:0.00 s:0.00 r:0.09
OK u:0.00 s:0.00 r:0.09
OK u:0.00 s:0.00 r:0.09
OK u:0.00 s:0.00 r:0.10
06/13/2015 05:16:53 PM - SYSTEM STATS: Time:0.2109 Method:cmd.php Processes:1 Threads:N/A Hosts:3 HostsPerProcess:3 DataSources:20 RRDsProcessed:17
```
成功添加！！