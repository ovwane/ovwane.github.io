---
title: uWSGI搭配Nginx配置Django部署环境
date: 2017-11-11 14:09
categories:
- 技术
- Linux
- Python
tags:
- CentOS
- Nginx
- uWSGI
- Python
- Django
---
[uWSGI搭配Nginx配置](http://www.nowamagic.net/academy/detail/1330308)Django部署环境

# WSGI简介
WSGI是什么？

WSGI，全称 Web Server Gateway Interface，或者 Python Web Server Gateway Interface ，是为 Python 语言定义的 Web 服务器和 Web 应用程序或框架之间的一种简单而通用的接口。自从 WSGI 被开发出来以后，许多其它语言中也出现了类似接口。

WSGI 的官方定义是，the Python Web Server Gateway Interface。从名字就可以看出来，这东西是一个Gateway，也就是网关。网关的作用就是在协议之间进行转换。

WSGI 是作为 Web 服务器与 Web 应用程序或应用框架之间的一种低级别的接口，以提升可移植 Web 应用开发的共同点。WSGI 是基于现存的 CGI 标准而设计的。

很多框架都自带了 WSGI server ，比如 Flask，webpy，Django、CherryPy等等。当然性能都不好，自带的 web server 更多的是测试用途，发布时则使用生产环境的 WSGI server或者是联合 nginx 做 uwsgi 。

WSGI的作用

WSGI有两方：“服务器”或“网关”一方，以及“应用程序”或“应用框架”一方。服务方调用应用方，提供环境信息，以及一个回调函数（提供给应用程序用来将消息头传递给服务器方），并接收Web内容作为返回值。

所谓的 WSGI中间件同时实现了API的两方，因此可以在WSGI服务和WSGI应用之间起调解作用：从WSGI服务器的角度来说，中间件扮演应用程序，而从应用程序的角度来说，中间件扮演服务器。“中间件”组件可以执行以下功能：

重写环境变量后，根据目标URL，将请求消息路由到不同的应用对象。
允许在一个进程中同时运行多个应用程序或应用框架。
负载均衡和远程处理，通过在网络上转发请求和响应消息。
进行内容后处理，例如应用XSLT样式表。
WSGI 的设计确实参考了 Java 的 servlet

WSGI是一种Web服务器网关接口。它是一个Web服务器（如nginx）与应用服务器（如uWSGI服务器）通信的一种规范。

接下来，我们要介绍的是 uWSGI。

uWSGI

uWSGI是一个Web服务器，它实现了WSGI协议、uwsgi、http等协议。Nginx中HttpUwsgiModule的作用是与uWSGI服务器进行交换。

要注意 WSGI / uwsgi / uWSGI 这三个概念的区分。

WSGI是一种通信协议。
uwsgi同WSGI一样是一种通信协议。
而uWSGI是实现了uwsgi和WSGI两种协议的Web服务器。
uwsgi协议是一个uWSGI服务器自有的协议，它用于定义传输信息的类型（type of information），每一个uwsgi packet前4byte为传输信息类型描述，它与WSGI相比是两样东西。

# Nginx 配置

在 nginx.conf 上加入/修改，我的 server 配置如下（一切从简……）：

```
server {
    listen       80;
    server_name  115.28.0.89;
    #server_name localhost;

    access_log /home/nowamagic/logs/access.log;
    error_log /home/nowamagic/logs/error.log;

    #root         /root/nowamagic_venv/nowamagic_pj;
    location / {
        uwsgi_pass 127.0.0.1:8077;
        #include uwsgi_params;
        include /etc/nginx/uwsgi_params;
        #uwsgi_pass 127.0.0.1:8077;
        #uwsgi_param UWSGI_SCRIPT index;
        #uwsgi_param UWSGI_PYHOME $document_root;
        #uwsgi_param UWSGI_CHDIR  $document_root;
   }
   access_log off;
}
```

注意保证配置里写的目录 /home/nowamagic/logs/ 和 /home/nowamagic/logs/ 存在，接下来就没啥问题了，Nginx 配置很简单。

# uWSGI 配置
## 安装uWSGI
```
pip install uwsgi
```

在实际部署环境中，我们常用的是配置文件的方式，而非命令行的方式。

我的 Django 程序目录：/root/nowamagic_venv/nowamagic_pj/

这里让 Nginx 采用 8077 端口与 uWSGI 通讯，请确保此端口没有被其它程序采用。

uWSGI 支持多种配置文件格式，比如 xml，ini，json 等等都可以。

xml

```
vi /root/nowamagic_venv/nowamagic_pj/nowamgic_pj.xml

<uwsgi>
 <socket>127.0.0.1:8077</socket>
 <listen>80</listen>
 <master>true</master>
 <pythonpath>/root/nowamagic_venv/nowamagic_pj</pythonpath>
 <processes>1</processes>
 <logdate>true</logdate>
 <daemonize>/var/log/uwsgi.log</daemonize>
 <plugins>python</plugins>
</uwsgi>
```

然后执行命令：

```
/usr/local/bin/uwsgi -x /root/nowamagic_venv/nowamagic_pj/nowamagic_pj.xml
```

ini 配置

```
[uwsgi]
vhost = false
plugins = python
socket = 127.0.0.1:8077
master = true
enable-threads = true
workers = 1
wsgi-file = /root/nowamagic_venv/nowamagic_pj/nowamagic_pj/wsgi.py
virtualenv = /root/nowamagic_venv
chdir = /root/nowamagic_venv/nowamagic_pj
```

执行命令：

```
uwsgi --ini /root/nowamagic_venv/nowamagic_pj.ini&
```

uwsgi 这样就启动起来了。如果无意外的话，就能在网上访问你的 Python 项目了。

一些我在配置时用到的命令，省得你去搜索：

关闭 uWSGI：

```
killall  -9 uwsgi
killall -s HUP /var/www/uwsgi 
killall -s HUP /usr/local/bin/uwsgi
```

列出端口占用情况：

```
netstat -lpnt
```