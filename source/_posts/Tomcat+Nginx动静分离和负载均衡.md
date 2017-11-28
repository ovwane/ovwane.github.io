---
title: Tomcat+Nginx动静分离和负载均衡
date: 2016-12-19 14:23:50
categories:
- 技术
- Linux
tags:
- CentOS
- Nginx
- Tomcat
---
[nginx反向代理tomcat集群实现动静分离](http://quenlang.blog.51cto.com/4813803/1570477)

我们都知道，nginx作为一个轻量级的web服务器，其在高并发下处理静态页面的优越性能是tomcat这样的web容器所无法媲美的，tomcat更倾向于处理动态文件，所以一个web应用可以通过nginx反向代理来实现动静分离，静态文件由nginx处理，动态文件由tomcat处理。

## 环境
环境：
    hadoop0.updb.com    192.168.0.100    nginx server
    hadoop2.updb.com    192.168.0.102    tomcat server
    hadoop3.updb.com    192.168.0.103    tomcat server
    hadoop4.updb.com    192.168.0.104    tomcat server
    hadoop5.updb.com    192.168.0.105    tomcat server
    
操作系统：
    centos
    
Nginx版本：
    nginx-1.7.6.tar.gz，采用源码编译安装
    
Tomcat版本：
    apache-tomcat-7.0.56.tar.gz
    
JDK版本：
    jdk-7u60-linux-x64.rpm
    
最终架构：
client->Nginx反向代理动静分离->{tomcat1,tomcat2,tomcat3,tomcat4}

# 配置
在弄明白架构之后，我们开始着手一步步来实现：
1、安装jdk + tomcat
hadoop2、hadoop3、hadoop4、hadoop5上安装jdk

```
rpm -ivh jdk-7u60-linux-x64.rpm
```

配置环境变量

```
[root@hadoop2 ~]# cat  .bash_profile 
# .bash_profile
 
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi
 
# User specific environment and startup programs
PATH=$PATH:$HOME/bin
export JAVA_HOME=/usr/java/jdk1.7.0_60
export JRE_HOME=/usr/java/jdk1.7.0_60/jre
export CLASSPATH=./:/usr/java/jdk1.7.0_60/lib:/usr/java/jdk1.7.0_60/jre/lib
export PATH
```

使环境变量生效，并验证java环境是否安装成功

```
[root@hadoop2 ~]# . .bash_profile 
[root@hadoop2 ~]# java -version
```

hadoop2、 hadoop3、hadoop4、hadoop5上安装tomcat，在webapps下创建测试目录shop和测试文件test.html和 test.jsp，测试文件中的内容为每个tomcat节点的主机名和IP地址，方便后边测试负载均衡。

```
[root@hadoop2 ~]# tar xf  apache-tomcat-7.0.56.tar.gz -C /opt/
[root@hadoop2 ~]# cd /opt/apache-tomcat-7.0.56/webapps/
[root@hadoop2 webapps]# mkdir shop
[root@hadoop2 webapps]# vi shop/test.html 
this is hadoop2 root`s html!
[root@hadoop2 webapps]# vi shop/test.jsp
this is hadoop2 root`s jsp!
```

将tomcat的bin目录配置到环境变量，并使更改生效，并启动tomcat

```
## 设置环境变量
[root@hadoop2 ~]# cat .bash_profile 
# .bash_profile
 
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi
 
# User specific environment and startup programs
PATH=$PATH:$HOME/bin:/opt/apache-tomcat-7.0.56/bin
export JAVA_HOME=/usr/java/jdk1.7.0_60
export JRE_HOME=/usr/java/jdk1.7.0_60/jre
export CLASSPATH=./:/usr/java/jdk1.7.0_60/lib:/usr/java/jdk1.7.0_60/jre/lib
export PATH
 
## 使设置生效
[root@hadoop2 ~]# . .bash_profile
## 启动tomcat
[root@hadoop2 ~]# /opt/apache-tomcat-7.0.56/bin/startup.sh 
```

测试tomcat是否正常工作，浏览器中访问，能够正常显示测试页面，表明工作正常。
http://IP:8080

2、安装nginx

```
[root@hadoop0 ~]# tar  xf nginx-1.7.6.tar.gz  -C /opt/
[root@hadoop0 ~]# cd  /opt/nginx-1.7.6/
[root@hadoop0 ~]# ./configure --user=www --group=www --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
[root@hadoop0 ~]# make && make install
```
关于源码安装nginx，比较简单，如果过程中遇到问题可以到网上看看，都能找到答案，也可以在下面留言，一起讨论。

为了方便管理，为nginx编写服务脚本
```
[root@hadoop0 ~]# vi /etc/init.d/nginx 
#!/bin/bash
#
#chkconfig: - 85 15
#description: this script use to manage nginx process.
#
 
#set -x
. /etc/rc.d/init.d/functions
 
procnum=`ps -ef |grep "/usr/local/nginx/sbin/nginx"|grep -v "grep"|wc -l`
 
start () {
        if [ "$procnum" -eq 1 -a -f /usr/local/nginx/logs/nginx.pid ]; then
            echo -n "Starting nginx:"
            success
            echo
        else
            /usr/local/nginx/sbin/nginx
            if [ "$?" -eq 0 ]; then 
                echo -n "Starting nginx:"
                success
                echo
            else
                echo -n "Starting nginx:"
                failure
                echo
                exit 4
            fi
        fi
}
 
stop () {
        if [ "$procnum" -eq 1 -a -f /usr/local/nginx/logs/nginx.pid ]; then
            /usr/local/nginx/sbin/nginx -s stop
            if [ "$?" -eq 0 ]; then
                    echo -n "Stopping nginx:"
                    success
                    echo
            else 
                    echo -n "Stopping nginx:"
                    failure
                    echo
                    exit 3
            fi
        else  
            echo -n "Stopping nginx:"
            success
            echo
        fi
}
 
case $1 in
 
    start)
        start
        ;;
 
    stop)
        stop
        ;;
 
    restart)
        stop
        sleep 1
        start
        ;;
 
    reload)
        if [ "$procnum" -eq 1 -a -f /usr/local/nginx/logs/nginx.pid ]; then
            /usr/local/nginx/sbin/nginx -s reload
        else 
            echo "nginx is not running!please start nginx first..."
            exit 2
        fi
        ;;
 
    status)
        if [ "$procnum" -eq 1 -a -f /usr/local/nginx/logs/nginx.pid ]; then
            echo "nginx is running..."
        else 
            echo "nginx is not running..."
        fi
        ;;
 
        *)
        echo "Usage : nginx [ start|stop|reload|restart|status ]"
        exit 1
        ;;
esac
```

然后授予脚本可执行权限，并加入chkconfig开机自启动，并测试

```
[root@hadoop0 ~]# chmod +x /etc/init.d/nginx
[root@hadoop0 ~]# chkconfig nginx on
[root@hadoop0 ~]# /etc/init.d/nginx start
Starting nginx:                                            [  OK  ]
[root@hadoop0 ~]# /etc/init.d/nginx status
nginx is running...
[root@hadoop0 ~]# /etc/init.d/nginx stop
Stopping nginx:                                            [  OK  ]
```

3、配置nginx，实现反向代理和动静分离

```
[root@hadoop0 ~]# cat /usr/local/nginx/conf/nginx.conf
user  www www;
worker_processes  8;
 
error_log  /usr/local/nginx/logs/error.log crit;
 
pid        /usr/local/nginx/logs/nginx.pid;
 
 
events {
    use epoll;
    worker_connections  65535;
}
 
 
http {
    include       mime.types;
    default_type  application/octet-stream;
    access_log  /usr/local/nginx/logs/access.log;
    charset utf-8;
    sendfile        on;
    tcp_nopush     on;
    keepalive_timeout  60;
 
    client_body_buffer_size  512k;
    proxy_connect_timeout    5;
    proxy_read_timeout       60;
    proxy_send_timeout       5;
    proxy_buffer_size        16k;
    proxy_buffers            4 64k;
    proxy_busy_buffers_size 128k;
    proxy_temp_file_write_size 128k;
 
    gzip  on;
    gzip_min_length  1k;
    gzip_buffers     4 16k;
    gzip_http_version 1.1;
    gzip_comp_level 2;
    gzip_types       text/plain application/x-javascript text/css application/xml;
    gzip_vary on;
     
    ## 配置反向代理的后端tomcat集群
    upstream web_server {
        server 192.168.0.102:8080 weight=1 max_fails=2 fail_timeout=30s;
        server 192.168.0.103:8080 weight=1 max_fails=2 fail_timeout=30s;
        server 192.168.0.104:8080 weight=1 max_fails=2 fail_timeout=30s;
        server 192.168.0.105:8080 weight=1 max_fails=2 fail_timeout=30s;
    }
 
    server {
        listen       80;
        server_name  192.168.0.100;
        root   html;
        index  index.html index.htm;
         
        ## 网页、视频、图片文件从本地读取，且定义在浏览器中缓存30天
        location ~ .*\.(htm|html|gif|jpg|jpeg|png|bmp|swf|ioc|rar|zip|txt|flv|mid|doc|ppt|pdf|xls|mp3|wma)$
        {       
            expires 30d;    
        }      
        ## js、css文件从本地读取，且定义在浏览器中缓存1小时
        location ~ .*\.(js|css)?$     
        {       
            expires 1h;
        }      
         
        ## 动态文件转发到后端的tomcat集群
        location ~ .*\.(php|jsp|cgi|jhtml)?$ {
            proxy_pass http://web_server;
            proxy_set_header Host  $host;
            proxy_set_header X-Forwarded-For  $proxy_add_x_forwarded_for;
            proxy_set_header X-Real-IP  $remote_addr;
        }
        ## 关闭访问日志
        access_log off;
    }
}
```

4、测试反向代理的负载均衡和动静分离
在nginx的html目录下创建测试目录shop、测试文件test.html和test.jsp

```
[root@hadoop0 ~]# cd /usr/local/nginx/html/
[root@hadoop0 html]# mkdir shop
[root@hadoop0 html]# vi shop/test.jsp 
this is nginx root`s jsp!
[root@hadoop0 html]# vi shop/test.html
this is nginx root`s html!
```

在浏览器中访问http://192.168.0.100/shop/test.html ，无论怎样刷新，页面都显示如下：
>this is nginx root's html!

    在浏览器中访问http://192.168.0.100/shop/test.jsp ，刷新几次，页面显示结果依次如下：
>this is hadoop2 root's jsp!
>this is hadoop3 root's jsp!
>this is hadoop4 root's jsp!
>this is hadoop5 root's jsp!

    从结果来看，访问html静态文件时，返回的是nginx中的文件，而访问jsp动态页面时则是轮询后端的tomcat集群。至此，反向代理+动静分离已经实现。

5、接着我们来比较动静分离与单纯的反向代理的性能差异

首先安装模拟并发访问的压力测试工具

```
yum install httpd-tools -y
```

首先测试访问nginx代理

```
[root@hadoop0 ~]# ab -n 20000 -c 3000 
```
一共请求20000次，每次3000的并发访问，总共用的时间为4.090秒，吞吐量为1.806M/s,再看直接访问tomcat的结果：

```
[root@hadoop0 ~]# ab -n 20000 -c 3000 http://192.168.0.105:8080/shop/test.html
```
相同压力下，访问tomcat需要8.591秒，且吞吐量只有0.67M/s，可见nginx在处理静态页面上远优于tomcat。

结束语：我在实现了动静分离之后，想通过nginx自带的proxy_cache模块来实现nginx缓存自身代理的静态页面，发现无法成功缓存到，认为porxy_cache无法缓存作为静态服务器的nginx中的文件，但无法求证，如果您知道，拜托指点一二。拜谢!

# 前端nginx时，让后端tomcat记录真实IP
[前端nginx时，让后端tomcat记录真实IP](http://sofar.blog.51cto.com/353572/1712069)
对于nginx+tomcat这种架构，如果后端tomcat配置保持默认，那么tomcat的访问日志里，记录的就是前端nginx的IP地址，而不是真实的访问IP。因此，需要对nginx、tomcat做如下配置：

1）nginx配置

```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```

2）tomcat配置
```
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="/data/logs/tomcat"
               prefix="search_access" suffix=".log"
               pattern="%{X-FORWARDED-FOR}i %l %u %t %r %s %b %D %{User-Agent}i" resolveHosts="false"/>
```

3）日志效果

```
10.0.11.162 - - [12/Nov/2015:11:22:11 +0800] GET /elesearch-search-api/search/restaurant.json?start=0&end=1&lbs=POINT%28120.285468902%2031.5033700783%29&rt=id&tq=id:328864 HTTP/1.0 200 102 2 python-requests/2.4.3 CPython/2.7.5 Linux/3.10.0-229.11.1.el7.x86_64
10.0.50.54 - - [12/Nov/2015:11:22:11 +0800] GET /elesearch-search-api/search/restaurantv5home.json?rt=id%2Cdelivery_price&lbs=POINT%28120.387601+36.273438%29&end=500&uid=22454922&start=0&from=webapi HTTP/1.0 200 6404 11 python-requests/2.7.0 CPython/2.7.5 Linux/3.10.0-229.el7.x86_64
10.0.40.91 - - [12/Nov/2015:11:22:11 +0800] GET /elesearch-search-api/search/restaurant.json?start=0&end=1&lbs=POINT%28116.316306833%2039.9776800442%29&rt=id&tq=id:376608 HTTP/1.0 200 119 3 python-requests/2.4.3 CPython/2.7.5 Linux/3.10.0-229.11.1.el7.x86_64
```

现在我们主要来看一下pattern配置段，它用于指定日志的输出格式。有效的日志格式模式可以参见下面内容，如下字符串，其对应的信息由指定的响应内容取代：

```
％a - 远程IP地址
％A - 本地IP地址
％b - 发送的字节数，不包括HTTP头，或“ - ”如果没有发送字节
％B - 发送的字节数，不包括HTTP头
％h - 远程主机名
％H - 请求协议
％l (小写的L)- 远程逻辑从identd的用户名（总是返回' - '）
％m - 请求方法
％p - 本地端口
％q - 查询字符串（在前面加上一个“？”如果它存在，否则是一个空字符串
％r - 第一行的要求
％s - 响应的HTTP状态代码
％S - 用户会话ID
％t - 日期和时间，在通用日志格式
％u - 远程用户身份验证
％U - 请求的URL路径
％v - 本地服务器名
％D - 处理请求的时间（以毫秒为单位）
％T - 处理请求的时间（以秒为单位）
％I （大写的i） - 当前请求的线程名称
```

此外，您可以指定以下别名来设置为普遍使用的模式之一：
common - %h %l %u %t "%r" %s %b
combined - %h %l %u %t "%r" %s %b "%{Referer}i" "%{User-Agent}i" 

另外，还可以将request请求的查询参数、session会话变量值、cookie值或HTTP请求/响应头内容的变量值等内容写入到日志文件。
它仿照了apache的语法：

```
％{XXX}i xxx代表传入的头(HTTP Request)
％{XXX}o xxx代表传出的响应头(Http Resonse)
％{XXX}c  xxx代表特定的Cookie名
％{XXX}r  xxx代表ServletRequest属性名
％{XXX}s xxx代表HttpSession中的属性名
```