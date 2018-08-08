---
date: 2018-08-08 20:37:00
---

### 安装git
```
apt-get install -y git
```

### 下载源码
```
git clone -b manyuser https://github.com/mengskysama/shadowsocks.git
git clone https://github.com/wzxjohn/moeSS.git
```

### 配置LAMP环境
http2.4
php5.4+
mysql5.6

```
#安装apache httpd web服务
apt-get install -y apache2

#设置开机自启动
systemctl enable apache2
#启动应用
systemctl start apache2

#开启rewrite功能
a2enmod rewrite
systemctl restart apache2
```

修改配置文件

```
vim /etc/apache2/apache2.conf
/var/www/html/ #此路径为web服务器存放网页的地方。
AllowOverride All #开启rewrite所有
```

### 安装mysql数据库
```
apt-get install -y mysql-server

#开机自启动
systemctl enable mysql
systemctl start mysql

#新建数据库 shaodowsocks
create database shadowsocks;
#导入数据库结构表。
mysql -uroot -p shadowsocks <shadowsocks.sql
```

### 安装php和php插件
```
apt-get -y install php5 libapache2-mod-php5 php5-mysql php5-mysqlnd php5-curl php5-gd php5-intl php-pear php5-imagick php5-imap php5-mcrypt php5-memcache php5-pspell php5-recode php5-snmp php5-sqlite php5-tidy php5-xmlrpc php5-xsl

#重启web服务
systemctl restart apache2
```

### 安装shadowsocks 插件
```
#安装pip工具，
apt-get install -y python-pip python-m2crypto

#安装python数据库支持
pip install cymysql

#运行shadowsocks
cd shadowsocks 
nohup python server.py &
```