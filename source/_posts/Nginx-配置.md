---
title: Nginx 配置
date: 2013-04-20 09:22:28
tags:
- Nginx
---

# [Nginx](https://nginx.org/)



## 介绍

Nginx 是一个自由、开源、高性能及轻量级的HTTP服务器及反转代理服务器，
其性能与IMAP/POP3代理服务器相当。Nginx以其高性能、稳定、功能丰富、配置简单及占用系统资源少而著称。
Nginx 超越 Apache 的高性能和稳定性，使得国内使用 Nginx 作为 Web 服务器的网站也越来越多.



### 基础功能

- 处理静态文件，索引文件以及自动索引； 
- 反向代理加速(无缓存)，简单的负载均衡和容错；
- FastCGI，简单的负载均衡和容错；
- 模块化的结构。过滤器包括gzipping, byte ranges, chunked responses, 以及 SSI-filter 。在SSI过滤器中，到同一个 proxy 或者 FastCGI 的多个子请求并发处理；
- SSL 和 TLS SNI 支持；



<!--more-->



### 优势

- Nginx专为性能优化而开发，性能是其最重要的考量, 实现上非常注重效率 。它支持内核Poll模型，能经受高负载的考验, 有报告表明能支持高达 50,000 个并发连接数。 
Nginx作为负载均衡服务器: Nginx 既可以在内部直接支持 Rails 和 PHP 程序对外进行服务, 也可以支持作为 HTTP代理服务器对外进行服务。
Nginx具有很高的稳定性。其它HTTP服务器，当遇到访问的峰值，或者有人恶意发起慢速连接时，也很可能会导致服务器物理内存耗尽频繁交换，失去响应，只能重启服务器。
- 例如当前apache一旦上到200个以上进程，web响应速度就明显非常缓慢了。而Nginx采取了分阶段资源分配技术，使得它的CPU与内存占用率非常低。
nginx官方表示保持10,000个没有活动的连接，它只占2.5M内存，就稳定性而言, nginx比lighthttpd更胜一筹。 
Nginx支持热部署。它的启动特别容易, 并且几乎可以做到7*24不间断运行，即使运行数个月也不需要重新启动。你还能够在不间断服务的情况下，对软件版本进行进行升级。 
Nginx采用C进行编写, 不论是系统资源开销还是CPU使用效率都比 Perlbal 要好很多。



## 安装

### Docker

```
docker pull nginx:1.17.9
```

```
docker run --name nginx -p 80:80 -p 443:443 -v ${HOME}/docker/nginx/conf.d:/etc/nginx/conf.d:ro -v ${HOME}/docker/nginx/html:/usr/share/nginx/html:ro -d nginx:1.17.9
```



### macOS

```shell
# 安装
brew install nginx
# 启动
brew services start nginx
```

文件位置：`/usr/local/etc/nginx/servers/`

HTML 位置：`/usr/local/var/www`



### 源码安装

#### 安装依赖
安装pcre，为了支持rewrite功能，我们需要安装pcre。
```
yum install -y pcre*
```

安装openssl，需要ssl的支持，如果不需要ssl支持，请跳过这一步。
```
yum install -y openssl*
```

安装make工具

```
yum install make -y
```

安装 Nginx

```
wget http://nginx.org/download/nginx-1.5.1.tar.gz
tar xvf nginx-1.5.1.tar.gz
cd nginx-1.5.1
```

编译参数

```
--prefix= 指向安装目录
--sbin-path 指向（执行）程序文件（nginx）
--conf-path= 指向配置文件（nginx.conf）
--error-log-path= 指向错误日志目录
--pid-path= 指向pid文件（nginx.pid）
--lock-path= 指向lock文件（nginx.lock）（安装文件锁定，防止安装文件被别人利用，或自己误操作。）
--user= 指定程序运行时的非特权用户
--group= 指定程序运行时的非特权用户组
--builddir= 指向编译目录
--with-rtsig_module 启用rtsig模块支持（实时信号）
--with-select_module 启用select模块支持（一种轮询模式,不推荐在高载环境下使用）禁用：--without-select_module
--with-poll_module 启用poll模块支持（功能与select相同，与select特性相同，为一种轮询模式,不推荐在高载环境下使用）
--with-file-aio 启用file aio支持（一种APL文件传输格式）
--with-ipv6 启用ipv6支持
--with-http_ssl_module 启用ngx_http_ssl_module支持（使支持https请求，需已安装openssl）
--with-http_realip_module 启用ngx_http_realip_module支持（这个模块允许从请求标头更改客户端的IP地址值，默认为关）
--with-http_addition_module 启用ngx_http_addition_module支持（作为一个输出过滤器，支持不完全缓冲，分部分响应请求）
--with-http_xslt_module 启用ngx_http_xslt_module支持（过滤转换XML请求）
--with-http_image_filter_module 启用ngx_http_image_filter_module支持（传输JPEG/GIF/PNG 图片的一个过滤器）（默认为不启用。gd库要用到）
--with-http_geoip_module 启用ngx_http_geoip_module支持（该模块创建基于与MaxMind GeoIP二进制文件相配的客户端IP地址的ngx_http_geoip_module变量）
--with-http_sub_module 启用ngx_http_sub_module支持（允许用一些其他文本替换nginx响应中的一些文本）
--with-http_dav_module 启用ngx_http_dav_module支持（增加PUT,DELETE,MKCOL：创建集合,COPY和MOVE方法）默认情况下为关闭，需编译开启
--with-http_flv_module 启用ngx_http_flv_module支持（提供寻求内存使用基于时间的偏移量文件）
--with-http_gzip_static_module 启用ngx_http_gzip_static_module支持（在线实时压缩输出数据流）
--with-http_random_index_module 启用ngx_http_random_index_module支持（从目录中随机挑选一个目录索引）
--with-http_secure_link_module 启用ngx_http_secure_link_module支持（计算和检查要求所需的安全链接网址）
--with-http_degradation_module  启用ngx_http_degradation_module支持（允许在内存不足的情况下返回204或444码）
--with-http_stub_status_module 启用ngx_http_stub_status_module支持（获取nginx自上次启动以来的工作状态）
--without-http_charset_module 禁用ngx_http_charset_module支持（重新编码web页面，但只能是一个方向--服务器端到客户端，并且只有一个字节的编码可以被重新编码）
--without-http_gzip_module 禁用ngx_http_gzip_module支持（该模块同-with-http_gzip_static_module功能一样）
--without-http_ssi_module 禁用ngx_http_ssi_module支持（该模块提供了一个在输入端处理处理服务器包含文件（SSI）的过滤器，目前支持SSI命令的列表是不完整的）
--without-http_userid_module 禁用ngx_http_userid_module支持（该模块用来处理用来确定客户端后续请求的cookies）
--without-http_access_module 禁用ngx_http_access_module支持（该模块提供了一个简单的基于主机的访问控制。允许/拒绝基于ip地址）
--without-http_auth_basic_module禁用ngx_http_auth_basic_module（该模块是可以使用用户名和密码基于http基本认证方法来保护你的站点或其部分内容）
--without-http_autoindex_module 禁用disable ngx_http_autoindex_module支持（该模块用于自动生成目录列表，只在ngx_http_index_module模块未找到索引文件时发出请求。）
--without-http_geo_module 禁用ngx_http_geo_module支持（创建一些变量，其值依赖于客户端的IP地址）
--without-http_map_module 禁用ngx_http_map_module支持（使用任意的键/值对设置配置变量）
--without-http_split_clients_module 禁用ngx_http_split_clients_module支持（该模块用来基于某些条件划分用户。条件如：ip地址、报头、cookies等等）
--without-http_referer_module 禁用disable ngx_http_referer_module支持（该模块用来过滤请求，拒绝报头中Referer值不正确的请求）
--without-http_rewrite_module 禁用ngx_http_rewrite_module支持（该模块允许使用正则表达式改变URI，并且根据变量来转向以及选择配置。如果在server级别设置该选项，那么他们将在 location之前生效。如果在location还有更进一步的重写规则，location部分的规则依然会被执行。如果这个URI重写是因为location部分的规则造成的，那么 location部分会再次被执行作为新的URI。 这个循环会执行10次，然后Nginx会返回一个500错误。）
--without-http_proxy_module 禁用ngx_http_proxy_module支持（有关代理服务器）
--without-http_fastcgi_module 禁用ngx_http_fastcgi_module支持（该模块允许Nginx 与FastCGI 进程交互，并通过传递参数来控制FastCGI 进程工作。 ）FastCGI一个常驻型的公共网关接口。
--without-http_uwsgi_module 禁用ngx_http_uwsgi_module支持（该模块用来医用uwsgi协议，uWSGI服务器相关）
--without-http_scgi_module 禁用ngx_http_scgi_module支持（该模块用来启用SCGI协议支持，SCGI协议是CGI协议的替代。它是一种应用程序与HTTP服务接口标准。它有些像FastCGI但他的设计 更容易实现。）
--without-http_memcached_module 禁用ngx_http_memcached_module支持（该模块用来提供简单的缓存，以提高系统效率）
-without-http_limit_zone_module 禁用ngx_http_limit_zone_module支持（该模块可以针对条件，进行会话的并发连接数控制）
--without-http_limit_req_module 禁用ngx_http_limit_req_module支持（该模块允许你对于一个地址进行请求数量的限制用一个给定的session或一个特定的事件）
--without-http_empty_gif_module 禁用ngx_http_empty_gif_module支持（该模块在内存中常驻了一个1*1的透明GIF图像，可以被非常快速的调用）
--without-http_browser_module 禁用ngx_http_browser_module支持（该模块用来创建依赖于请求报头的值。如果浏览器为modern ，则$modern_browser等于modern_browser_value指令分配的值；如 果浏览器为old，则$ancient_browser等于 ancient_browser_value指令分配的值；如果浏览器为 MSIE中的任意版本，则 $msie等于1）
--without-http_upstream_ip_hash_module 禁用ngx_http_upstream_ip_hash_module支持（该模块用于简单的负载均衡）
--with-http_perl_module 启用ngx_http_perl_module支持（该模块使nginx可以直接使用perl或通过ssi调用perl）
--with-perl_modules_path= 设定perl模块路径
--with-perl= 设定perl库文件路径
--http-log-path= 设定access log路径
--http-client-body-temp-path= 设定http客户端请求临时文件路径
--http-proxy-temp-path= 设定http代理临时文件路径
--http-fastcgi-temp-path= 设定http fastcgi临时文件路径
--http-uwsgi-temp-path= 设定http uwsgi临时文件路径
--http-scgi-temp-path= 设定http scgi临时文件路径
-without-http 禁用http server功能
--without-http-cache 禁用http cache功能
--with-mail 启用POP3/IMAP4/SMTP代理模块支持
--with-mail_ssl_module 启用ngx_mail_ssl_module支持
--without-mail_pop3_module 禁用pop3协议（POP3即邮局协议的第3个版本,它是规定个人计算机如何连接到互联网上的邮件服务器进行收发邮件的协议。是因特网电子邮件的第一个离线协议标 准,POP3协议允许用户从服务器上把邮件存储到本地主机上,同时根据客户端的操作删除或保存在邮件服务器上的邮件。POP3协议是TCP/IP协议族中的一员，主要用于 支持使用客户端远程管理在服务器上的电子邮件）
--without-mail_imap_module 禁用imap协议（一种邮件获取协议。它的主要作用是邮件客户端可以通过这种协议从邮件服务器上获取邮件的信息，下载邮件等。IMAP协议运行在TCP/IP协议之上， 使用的端口是143。它与POP3协议的主要区别是用户可以不用把所有的邮件全部下载，可以通过客户端直接对服务器上的邮件进行操作。）
--without-mail_smtp_module 禁用smtp协议（SMTP即简单邮件传输协议,它是一组用于由源地址到目的地址传送邮件的规则，由它来控制信件的中转方式。SMTP协议属于TCP/IP协议族，它帮助每台计算机在发送或中转信件时找到下一个目的地。）
--with-google_perftools_module 启用ngx_google_perftools_module支持（调试用，剖析程序性能瓶颈）
--with-cpp_test_module 启用ngx_cpp_test_module支持
--add-module= 启用外部模块支持
--with-cc= 指向C编译器路径
--with-cpp= 指向C预处理路径
--with-cc-opt= 设置C编译器参数（PCRE库，需要指定–with-cc-opt=”-I /usr/local/include”，如果使用select()函数则需要同时增加文件描述符数量，可以通过–with-cc- opt=”-D FD_SETSIZE=2048”指定。）
--with-ld-opt= 设置连接文件参数。（PCRE库，需要指定–with-ld-opt=”-L /usr/local/lib”。）
--with-cpu-opt= 指定编译的CPU，可用的值为: pentium, pentiumpro, pentium3, pentium4, athlon, opteron, amd64, sparc32, sparc64, ppc64
--without-pcre 禁用pcre库
--with-pcre 启用pcre库
--with-pcre= 指向pcre库文件目录
--with-pcre-opt= 在编译时为pcre库设置附加参数
--with-md5= 指向md5库文件目录（消息摘要算法第五版，用以提供消息的完整性保护）
--with-md5-opt= 在编译时为md5库设置附加参数
--with-md5-asm 使用md5汇编源
--with-sha1= 指向sha1库目录（数字签名算法，主要用于数字签名）
--with-sha1-opt= 在编译时为sha1库设置附加参数
--with-sha1-asm 使用sha1汇编源
--with-zlib= 指向zlib库目录
--with-zlib-opt= 在编译时为zlib设置附加参数
--with-zlib-asm= 为指定的CPU使用zlib汇编源进行优化，CPU类型为pentium, pentiumpro
--with-libatomic 为原子内存的更新操作的实现提供一个架构
--with-libatomic= 指向libatomic_ops安装目录
--with-openssl= 指向openssl安装目录
--with-openssl-opt 在编译时为openssl设置附加参数
--with-debug 启用debug日志

--prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -pie'
```



编译

```
./configure --prefix=/usr/local/nginx \
--with-http_ssl_module \
--with-http_stub_status_module \
--with-pcre

make
make install
```



安装依赖环境

```bash
yum install -y make gcc git pcre-devel zlib-devel
```



添加nginx用户和组

```
useradd -s /sbin/nologin -M nginx 
```



获取源码：

```bash
git clone -b 1.13.12 https://github.com/nginx/nginx.git
git clone https://github.com/openssl/openssl.git
git clone https://github.com/grahamedgecombe/nginx-ct.git
git clone https://github.com/google/ngx_brotli.git&&cd ngx_brotli&&git submodule update --init&&cd ..
```



编译

```bash
cd nginx
#开启ipv6，但编译参数没有 --with-ipv6 应该默认支持ipv6了。
auto/configure --prefix=/usr/local/nginx-1.13.11 --user=nginx --group=nginx --add-module=../ngx_brotli --add-module=../nginx-ct --with-openssl=../openssl --with-openssl-opt='enable-tls1_3 enable-weak-ssl-ciphers' --with-http_v2_module --with-http_ssl_module --with-http_gzip_static_module

make
make install
```

nginx.service

```
cat >/usr/lib/systemd/system/nginx.service<<EOF
[Unit]
Description=nginx - high performance web server
Documentation=http://nginx.org/en/docs/
After=network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/var/run/nginx.pid
ExecStartPre=/usr/local/nginx-1.13.11/sbin/nginx -t -c /usr/local/nginx-1.13.11/conf/nginx.conf
ExecStart=/usr/local/nginx-1.13.11/sbin/nginx -c /usr/local/nginx-1.13.11/conf/nginx.conf
ExecReload=/usr/local/nginx-1.13.11/sbin/nginx -s reload
ExecStop=/usr/local/nginx-1.13.11/sbin/nginx -s quit
PrivateTmp=true

[Install]
WantedBy=multi-user.target
EOF
```



启动nginx

```shell
# 启动
nginx start
# 关闭
nginx stop
# 测试配置文件
nginx -t
# 重新加载配置文件
nginx -s reload
```



### yum 安装

添加 nginx.repo 源

```
cat >> /etc/yum.repos.d/nginx.repo <<EOF
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/mainline/centos/7/$basearch/
gpgcheck=0
enabled=1
EOF
```



安装 Nginx

```shell
yum install nginx -y
# 查看nginx版本
nginx -v
# 获取编译参数
nginx -V
```



启动

```bash
systemctl start nginx.service
systemctl restart nginx.service
systemctl stop nginx.service
systemctl status nginx.service
systemctl enable nginx.service
```



## Nginx 语法

### location

语法规则： location [=|~|~*|^~] /uri/ { … }

- = 开头表示精确匹配
- ^~ 开头表示uri以某个常规字符串开头，理解为匹配 url路径即可。nginx不对url做编码，因此请求为/static/20%/aa，可以被规则^~ /static/ /aa匹配到（注意是空格）。
- ~ 开头表示区分大小写的正则匹配
- ~*  开头表示不区分大小写的正则匹配
- !~和!~*分别为区分大小写不匹配及不区分大小写不匹配 的正则
- / 通用匹配，任何请求都会匹配到。

多个location配置的情况下匹配顺序为（参考资料而来，还未实际验证，试试就知道了，不必拘泥，仅供参考）：

首先匹配 =，其次匹配^~, 其次是按文件中顺序的正则匹配，最后是交给 / 通用匹配。当有匹配成功时候，停止匹配，按当前匹配规则处理请求。



- 写优先级 =(绝对匹配) > /url (真正全路径) > ^~(带开头的正则，和~ ^/url 一样) > ~和~* (模糊正则) > /url (非全路径) > / (匹配所有的) 其实就是 2 5 6 是一种，都是直接匹配路径，但一个比一个模糊 3,4是一种，都是正则，3比4更清晰，其他语言的正则里面也是3优先 1，就是绝对等于，相当于 ~ ^/xxx/xxx$ 完全固定不能匹配任何其他的。



```
    location /wechat/ {
        proxy_pass http://10.8.8.8:8080/;
    }
```

1. Nginx location 配置语法

   ```
       1. location [ = | ~ | ~* | ^~ ] uri { ... }
       2. location @name { ... }    
   ```

   1. location 配置可以有两种配置方法

      ```
      1.前缀 + uri（字符串/正则表达式）
      2.@ + name
      ```

   2. 前缀含义

      ```
          =  ：精确匹配（必须全部相等）
          ~  ：大小写敏感
          ~* ：忽略大小写
          ^~ ：只需匹配uri部分
          @  ：内部服务跳转
      ```

2. Location 基础知识

   1.location 是在 server 块中配置。
   2.可以根据不同的 URI 使用不同的配置（location 中配置），来处理不同的请求。
   3.location 是有顺序的，会被第一个匹配的location 处理。



### Location 配置demo

1.`=`，精确匹配

```
        location = / {
            #规则
        }
        # 则匹配到 `http://www.example.com/` 这种请求。 
```

2.`~`，大小写敏感

```
        location ~ /Example/ {
                #规则
        }
        #请求示例
        #http://www.example.com/Example/  [成功]
        #http://www.example.com/example/  [失败]
```

3.`~*`，大小写忽略

```
    location ~* /Example/ {
                #规则
    }
    # 则会忽略 uri 部分的大小写
    #http://www.example.com/Example/  [成功]
    #http://www.example.com/example/  [成功]
```

4.`^~`，只匹配以 uri 开头

```
    location ^~ /img/ {
            #规则
    }
    #以 /img/ 开头的请求，都会匹配上
    #http://www.example.com/img/a.jpg   [成功]
    #http://www.example.com/img/b.mp4 [成功]
```

5.`@`，nginx内部跳转

```
    location /img/ {
        error_page 404 @img_err;
    }
    
    location @img_err {
        # 规则
    }
    #以 /img/ 开头的请求，如果链接的状态为 404。则会匹配到 @img_err 这条规则上。
```

Nginx 中的 location 并没有想象中的很难懂，不必害怕。多找资料看看，多尝试。你就会有收获。



### ReWrite

last – 基本上都用这个Flag。

break – 中止Rewirte，不在继续匹配

redirect – 返回临时重定向的HTTP状态302

permanent – 返回永久重定向的HTTP状态301

注：last和break最大的不同在于

- -break是终止当前location的rewrite检测,而且不再进行location匹配 
- -last是终止当前location的rewrite检测,但会继续重试location匹配并处理区块中的rewrite规则

```
1、下面是可以用来判断的表达式：

-f和!-f用来判断是否存在文件

-d和!-d用来判断是否存在目录

-e和!-e用来判断是否存在文件或目录

-x和!-x用来判断文件是否可执行


2、下面是可以用作判断的全局变量

$args #这个变量等于请求行中的参数。

$content_length #请求头中的Content-length字段。

$content_type #请求头中的Content-Type字段。

$document_root #当前请求在root指令中指定的值。

$host #请求主机头字段，否则为服务器名称。

$http_user_agent #客户端agent信息

$http_cookie #客户端cookie信息

$limit_rate #这个变量可以限制连接速率。

$request_body_file #客户端请求主体信息的临时文件名。

$request_method #客户端请求的动作，通常为GET或POST。

$remote_addr #客户端的IP地址。

$remote_port #客户端的端口。

$remote_user #已经经过Auth Basic Module验证的用户名。

$request_filename #当前请求的文件路径，由root或alias指令与URI请求生成。

query_string #与args相同。

$scheme #HTTP方法（如http，https）。

$server_protocol #请求使用的协议，通常是HTTP/1.0或HTTP/1.1。

$server_addr #服务器地址，在完成一次系统调用后可以确定这个值。

$server_name #服务器名称。

$server_port #请求到达服务器的端口号。

$request_uri #包含请求参数的原始URI，不包含主机名，如：”/foo/bar.php?arg=baz”。

uri #不带请求参数的当前URI，uri不包含主机名，如”/foo/bar.html”。

document_uri #与uri相同。

例：http://localhost:88/test1/test2/test.php

$host：localhost

$server_port：88

$request_uri：http://localhost:88/test1/test2/test.php

$document_uri：/test1/test2/test.php

$document_root：D:\nginx/html

$request_filename：D:\nginx/html/test1/test2/test.php
```



## 配置

### nginx安全配置
隐藏Nginx版本号 `server_tokens off;`
```
http {
sendfile on;
tcp_nopush on;
keepalive_timeout 60;
tcp_nodelay on;
server_tokens off;
}
```

如果使用了php-fpm,需要修改fastcfi.conf或fcgi.conf

```
fastcgi_param SERVER_SOFTWARE nginx/$nginx_version;
改为：
fastcgi_param SERVER_SOFTWARE nginx;
```

```
在配置文件中小心使用"if"
它是重写模块的一部分，不应该在任何地方使用。
“if”声明是重写模块评估指令强制性的部分。换个说法，Nginx的配置一般来说是声明式的。在有些情况下，由于用户的需求，他们试图在一些非重写指令内使用“if”，这导致我们现在遇到的情况。大多数情况下都能正常工作，但…看上面提到的。
看起来唯一正确的解决方案是在非重写的指令内完全禁用“if”。这将更改现有的许多配置，所以还没有完成。IfIsEvil：http://wiki.nginx.org/IfIsEvil

将每个~ .php$请求转递给PHP
我们上周发布了这个流行指令的潜在安全漏洞介绍。即使文件名为hello.php.jpeg它也会匹配~ .php$这个正则而执行文件。
现在有两个解决上述问题的好方法。我觉得确保你不轻易执行任意代码的混合方法很有必要。

1 如果没找到文件时使用try_files和only(在所有的动态执行情况下都应该注意) 将它转递给运行PHP的FCGI进程。
2 确认php.ini文件中cgi.fix_pathinfo设置为0 (cgi.fix_pathinfo=0) 。这样确保PHP检查文件全名(当它在文件结尾没有发现.php它将忽略)
3 修复正则表达式匹配不正确文件的问题。现在正则表达式认为任何文件都包含".php"。在站点后加“if”确保只有正确的文件才能运行。将/location ~ .php$和location ~ ..*/.*.php$都设置为return 403;

禁用autoindex模块
这个可能在你使用的Nginx版本中已经更改了，如果没有的话只需在配置文件的location块中增加autoindex off;声明即可。

禁用服务器上的ssi (服务器端引用)
这个可以通过在location块中添加ssi off; 。

在配置文件中设置自定义缓存以限制缓冲区溢出攻击的可能性

client_body_buffer_size 1K;
client_header_buffer_size 1k;
client_max_body_size 1k;
large_client_header_buffers 2 1k;

将timeout设低来防止DOS攻击
所有这些声明都可以放到主配置文件中。

client_body_timeout 10;
client_header_timeout 10;
keepalive_timeout 5 5;
send_timeout 10;

限制用户连接数来预防DOS攻击

limit_zone slimits $binary_remote_addr 5m;
limit_conn slimits 5;

试着避免使用HTTP认证
HTTP认证默认使用crypt，它的哈希并不安全。如果你要用的话就用MD5（这也不是个好选择但负载方面比crypt好） 。

保持与最新的Nginx安全更新
```



/usr/local/nginx-1.13.11/conf/nginx.conf

```nginx
user  nginx nginx;

worker_processes  2;

pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    charset            utf-8;

    sendfile       on;

    tcp_nopush     on;
    tcp_nodelay    on;

    keepalive_timeout  60;

    gzip               on;
    gzip_vary          on;
    gzip_comp_level    6;
    gzip_buffers       16 8k;
    gzip_min_length    1000;
    gzip_proxied       any;
    gzip_disable       "msie6";
    gzip_http_version  1.0; 
    gzip_types         text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/javascript image/svg+xml;

    brotli             on;
    brotli_comp_level  6;
    brotli_types       text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/javascript image/svg+xml;

    include            /data/nginx/conf/*.conf;
} 
```



/data/nginx/conf/ovwane.com.conf

```shell
server {
    listen               443 ssl http2 fastopen=3 reuseport;
    listen               [::]:443 ssl http2 fastopen=3 reuseport;

    # 如果你使用了 Cloudflare 的 HTTP/2 + SPDY 补丁，记得加上 spdy
    # listen               443 ssl http2 spdy fastopen=3 reuseport;

    server_name          ovwane.com;
    server_tokens        off;

    ssl_ct               on;

    #RSA
    ssl_certificate     /data/ssl/ovwane.com/ovwane.com_rsa_fullchain.cer;
    ssl_certificate_key /data/ssl/ovwane.com/ovwane.com_rsa.key;
    ssl_ct_static_scts   /data/ssl/ovwane.com/scts;

    #ECC
    ssl_certificate     /data/ssl/ovwane.com/ovwane.com_ecc_fullchain.cer;
    ssl_certificate_key /data/ssl/ovwane.com/ovwane.com_ecc.key;
    ssl_ct_static_scts   /data/ssl/ovwane.com/scts;

    ssl_dhparam         /data/ssl/ovwane.com/dhparams.pem;

    ssl_ciphers                TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+ECDSA+AES128:EECDH+aRSA+AES128:RSA+AES128:EECDH+ECDSA+AES256:EECDH+aRSA+AES256:RSA+AES256:EECDH+ECDSA+3DES:EECDH+aRSA+3DES:RSA+3DES:!MD5;

    ssl_prefer_server_ciphers  on;

    ssl_protocols              TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # 增加 TLSv1.3

    ssl_session_cache          shared:SSL:50m;
    ssl_session_timeout        1d;
    ssl_session_tickets        on;

    #OCSP
    ssl_stapling               on;
    ssl_stapling_verify        on;
    #ssl_trusted_certificate

    resolver                 8.8.8.8 1.1.1.1  valid=300s;
    resolver_timeout         10s;

    access_log                 /data/nginx/log/ovwane.com.log;

    if ($request_method !~ ^(GET|HEAD|POST|OPTIONS)$ ) {
        return           444;
    }

    if ($host != 'ovwane.com' ) {
        rewrite          ^/(.*)$  https://ovwane.com/$1 permanent;
    }

    location / {
        add_header               Strict-Transport-Security "max-age=31536000; includeSubDomains; preload";
        add_header               X-Frame-Options deny;
        add_header               X-Content-Type-Options nosniff;
        add_header               Content-Security-Policy "default-src 'none'; script-src 'unsafe-inline' 'unsafe-eval' blob:https:; img-src data: https: http://ovwane.com; style-src 'unsafe-inline' https:; child-src https:; connect-src 'self' https://translate.googleapis.com; frame-src https://disqus.com";
        add_header               Public-Key-Pins 'pin-sha256="sRHdihwgkaib1P1gxX8HFszlD+7/gTfNvuAybgLPNis="; pin-sha256="YLh1dUR9y6Kja30RrAn7JKnbQG/uEtLMkBgFF2Fuihg="; max-age=2592000; includeSubDomains';
        add_header               Cache-Control no-cache;
        #QUIC
        add_header "alt-svc" "quic=\":444\"; ma=2592000; v=\"39\"";

        root /data/www/ovwane.com/public;
        index index.html index.htm;
    }
}

server {
    listen 80;
    listen [::]:80;
    server_name ovwane.com;
    server_tokens off;

    access_log        /dev/null;

    if ($request_method !~ ^(GET|HEAD|POST)$ ) {
        return        444;
    }

    location / {
        rewrite       ^/(.*)$ https://ovwane.com/$1 permanent;
    }
}
```



## HTTPS

### 使用[acme.sh](https://github.com/Neilpang/acme.sh)工具获取ssl证书

```
curl https://get.acme.sh | sh
```

```bash
#1. DNSPod API Key and ID
export DP_Id=""
export DP_Key=""

#2.
acme.sh --issue --dns dns_dp --nginx --keylength 4096 -d quanjinlong.cn -d www.quanjinlong.cn -d blog.quanjinlong.cn

#3.
acme.sh --issue --dns dns_dp --nginx --keylength 4096 -d quanjinlong.cn -d www.quanjinlong.cn -d blog.quanjinlong.cn --keylength ec-256

#4.
acme.sh --installcert -d quanjinlong.cn -d www.quanjinlong.cn -d blog.quanjinlong.cn \
        --key-file   /data/ssl/quanjinlong.cn/quanjinlong_cn_rsa.key \
        --fullchain-file /data/ssl/quanjinlong.cn/quanjinlong_cn_rsa_fullchain.cer \
        --reloadcmd  "systemctl restart nginx.service"
      
#4.        
acme.sh --install-cert -d quanjinlong.cn -d www.quanjinlong.cn -d blog.quanjinlong.cn \
--key-file       /data/ssl/quanjinlong.cn/quanjinlong_cn_rsa_key.pem  \
--fullchain-file /data/ssl/quanjinlong.cn/quanjinlong_cn_rsa_fullchain_cert.pem \
--reloadcmd     "systemctl restart nginx.service"
```

nginx开启https
nginx的https协议需要ssl模块的支持，我们在编译nginx时使用–with-http_ssl_module参数加入SSL模块。还需要服务器私钥，服务器证书。

检查Nginx的SSL模块是否安装

```
nginx -V |grep ssl
```

准备证书
[StartSSL](https://www.startcomca.com)

开启Nginx SSL

```
server {
listen       443;
ssl on;
ssl_certificate /data/cert/server.crt;
ssl_certificate_key /data/cert/server.key;
}
```

重启nginx

```
nginx -s reload
netstat -lntup|grep 443
```

配置重定向80端口转443端口

```
server {
listen 80;
rewrite ^(.*) https://$server_name$1 permanent;
}
```

[ssl 测试工具](https://www.ssllabs.com/ssltest/)

- TLS 1.3
[本博客开始支持 TLS 1.3](https://imququ.com/post/enable-tls-1-3.html)
[CentOS 7 编译安装nginx并启用TLS1.3](https://www.coldawn.com/tag/draft-23/)

- CT
[通过 nginx-ct 启用 CT](https://imququ.com/post/certificate-transparency.html#toc-2)

[Certificate Transparency Monitor](https://ct.grahamedgecombe.com/)

```bash
yum -y install golang

wget -O ct-submit.zip -c https://github.com/grahamedgecombe/ct-submit/archive/v1.1.2.zip
unzip ct-submit.zip
cd ct-submit-1.1.2
go build
```

```
#rsa
./ct-submit-1.1.2 ct.googleapis.com/icarus </data/ssl/quanjinlong.cn/quanjinlong_cn_rsa_fullchain_cert.pem >/data/ssl/quanjinlong.cn/scts/icarus.sct

./ct-submit-1.1.2 ct1.digicert-ct.com/log </data/ssl/quanjinlong.cn/quanjinlong_cn_rsa_fullchain_cert.pem >/data/ssl/quanjinlong.cn/scts/digicert.sct

./ct-submit-1.1.2 mammoth.ct.comodo.com </data/ssl/quanjinlong.cn/quanjinlong_cn_rsa_fullchain_cert.pem >/data/ssl/quanjinlong.cn/scts/comodo.sct

#ecc
./ct-submit-1.1.2 ct.googleapis.com/icarus </data/ssl/quanjinlong.cn/quanjinlong_cn_ecc_fullchain_cert.pem >/data/ssl/quanjinlong.cn/scts/icarus_ecc.sct

./ct-submit-1.1.2 ct1.digicert-ct.com/log </data/ssl/quanjinlong.cn/quanjinlong_cn_ecc_fullchain_cert.pem >/data/ssl/quanjinlong.cn/scts/digicert_ecc.sct

./ct-submit-1.1.2 mammoth.ct.comodo.com </data/ssl/quanjinlong.cn/quanjinlong_cn_ecc_fullchain_cert.pem >/data/ssl/quanjinlong.cn/scts/comodo_ecc.sct
```

- [HSTS]()

nginx server conf

```
add_header               Strict-Transport-Security "max-age=31536000; includeSubDomains; preload";
```

- [HKPK](https://gist.github.com/esurdam/ef72f1c47be7c074499cb920683bd307)

[Chain of Trust - Let's Encrypt - Free SSL/TLS Certificates](https://letsencrypt.org/certificates/)

```
wget -O lets-encrypt-x3-cross-signed.pem https://letsencrypt.org/certs/lets-encrypt-x3-cross-signed.pem.txt

wget -O lets-encrypt-x4-cross-signed.pem https://letsencrypt.org/certs/lets-encrypt-x4-cross-signed.pem.txt
```

```
openssl x509 -noout -in lets-encrypt-x3-cross-signed.pem -pubkey | \
openssl rsa -pubin -outform der | \
openssl dgst -sha256 -binary | \
base64
```

```
openssl x509 -noout -in lets-encrypt-x4-cross-signed.pem -pubkey | \
openssl rsa -pubin -outform der | \
openssl dgst -sha256 -binary | \
base64
```

nginx server conf

```
add_header Public-Key-Pins 'pin-sha256="YLh1dUR9y6Kja30RrAn7JKnbQG/uEtLMkBgFF2Fuihg="; pin-sha256="sRHdihwgkaib1P1gxX8HFszlD+7/gTfNvuAybgLPNis=="; max-age=2592000; includeSubDomains';
```

- [ssl_dhparam](https://weakdh.org/sysadmin.html)

```
openssl dhparam -out dhparams.pem 2048
```

nginx server conf

```
ssl_dhparam dhparams.pem
```

[imquu.com-本博客 Nginx 配置之完整篇](https://imququ.com/post/my-nginx-conf.html)




### 日志配置

> 日志对于统计排错来说非常有利的。
>
> nginx日志相关的配置如：access_log、log_format、open_log_file_cache、log_not_found、log_subrequest、rewrite_log、error_log。
>
> nginx有一个非常灵活的日志记录模式。每个级别的配置可以有各自独立的访问日志。日志格式通过log_format命令来定义。ngx_http_log_module是用来定义请求日志格式的。



#### access_log指令

```
语法: access_log path [format [buffer=size [flush=time]]];
access_log path format gzip[=level] [buffer=size] [flush=time];
access_log syslog:server=address[,parameter=value] [format];
access_log off;
默认值: access_log logs/access.log combined;
配置段: http, server, location, if in location, limit_except
gzip压缩等级。
buffer设置内存缓存区大小。
flush保存在缓存区中的最长时间。
不记录日志：access_log off;
使用默认combined格式记录日志：access_log logs/access.log 或 access_log logs/access.log combined;
```



#### log_format指令

```
语法: log_format name string …;
默认值: log_format combined “…”;
配置段: http
```



name表示格式名称，string表示等义的格式。log_format有一个默认的无需设置的combined日志格式，相当于apache的combined日志格式，如下所示：

```
log_format  combined  '$remote_addr - $remote_user  [$time_local]  '
                                   ' "$request"  $status  $body_bytes_sent  '
                                   ' "$http_referer"  "$http_user_agent" ';
log_format  combined  '$remote_addr - $remote_user  [$time_local]  '
                                   ' "$request"  $status  $body_bytes_sent  '
                                   ' "$http_referer"  "$http_user_agent" ';
```



如果nginx位于负载均衡器，squid，nginx反向代理之后，web服务器无法直接获取到客户端真实的IP地址了。 

```
$remote_addr获取反向代理的IP地址。反向代理服务器在转发请求的http头信息中，可以增加X-Forwarded-For信息，用来记录 客户端IP地址和客户端请求的服务器地址。

log_format  porxy  '$http_x_forwarded_for - $remote_user  [$time_local]  '
                             ' "$request"  $status $body_bytes_sent '
                             ' "$http_referer"  "$http_user_agent" ';
log_format  porxy  '$http_x_forwarded_for - $remote_user  [$time_local]  '
                             ' "$request"  $status $body_bytes_sent '
                             ' "$http_referer"  "$http_user_agent" ';
```



日志格式允许包含的变量注释如下：

```
$remote_addr, $http_x_forwarded_for 记录客户端IP地址
$remote_user 记录客户端用户名称
$request 记录请求的URL和HTTP协议
$status 记录请求状态
$body_bytes_sent 发送给客户端的字节数，不包括响应头的大小； 该变量与Apache模块mod_log_config里的“%B”参数兼容。
$bytes_sent 发送给客户端的总字节数。
$connection 连接的序列号。
$connection_requests 当前通过一个连接获得的请求数量。
$msec 日志写入时间。单位为秒，精度是毫秒。
$pipe 如果请求是通过HTTP流水线(pipelined)发送，pipe值为“p”，否则为“.”。
$http_referer 记录从哪个页面链接访问过来的
$http_user_agent 记录客户端浏览器相关信息
$request_length 请求的长度（包括请求行，请求头和请求正文）。
$request_time 请求处理时间，单位为秒，精度毫秒； 从读入客户端的第一个字节开始，直到把最后一个字符发送给客户端后进行日志写入为止。
$time_iso8601 ISO8601标准格式下的本地时间。
$time_local 通用日志格式下的本地时间。

$remote_addr, $http_x_forwarded_for 记录客户端IP地址
$remote_user 记录客户端用户名称
$request 记录请求的URL和HTTP协议
$status 记录请求状态
$body_bytes_sent 发送给客户端的字节数，不包括响应头的大小； 该变量与Apache模块mod_log_config里的“%B”参数兼容。
$bytes_sent 发送给客户端的总字节数。
$connection 连接的序列号。
$connection_requests 当前通过一个连接获得的请求数量。
$msec 日志写入时间。单位为秒，精度是毫秒。
$pipe 如果请求是通过HTTP流水线(pipelined)发送，pipe值为“p”，否则为“.”。
$http_referer 记录从哪个页面链接访问过来的
$http_user_agent 记录客户端浏览器相关信息
$request_length 请求的长度（包括请求行，请求头和请求正文）。
$request_time 请求处理时间，单位为秒，精度毫秒； 从读入客户端的第一个字节开始，直到把最后一个字符发送给客户端后进行日志写入为止。
$time_iso8601 ISO8601标准格式下的本地时间。
$time_local 通用日志格式下的本地时间。
```

>发送给客户端的响应头拥有“sent_http_”前缀。 比如$sent_http_content_range。



实例如下：

```
http {
	log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                                        '"$status" $body_bytes_sent "$http_referer" '
                                        '"$http_user_agent" "$http_x_forwarded_for" '
                                        '"$gzip_ratio" $request_time $bytes_sent $request_length';

	log_format srcache_log '$remote_addr - $remote_user [$time_local] "$request" '
                                '"$status" $body_bytes_sent $request_time $bytes_sent $request_length '
                                '[$upstream_response_time] [$srcache_fetch_status] [$srcache_store_status] [$srcache_expire]';

	open_log_file_cache max=1000 inactive=60s;

	server {
		server_name ~^(www\.)?(.+)$;
		access_log logs/$2-access.log main;
		error_log logs/$2-error.log;

		location /srcache {
			access_log logs/access-srcache.log srcache_log;
		}
	}
}
```



#### open_log_file_cache指令

```
语法: open_log_file_cache max=N [inactive=time] [min_uses=N] [valid=time];
open_log_file_cache off;
默认值: open_log_file_cache off;
配置段: http, server, location
```

对于每一条日志记录，都将是先打开文件，再写入日志，然后关闭。可以使用open_log_file_cache来设置日志文件缓存(默认是off)，格式如下：
参数注释如下：

```
max:设置缓存中的最大文件描述符数量，如果缓存被占满，采用LRU算法将描述符关闭。
inactive:设置存活时间，默认是10s
min_uses:设置在inactive时间段内，日志文件最少使用多少次后，该日志文件描述符记入缓存中，默认是1次
valid:设置检查频率，默认60s
off：禁用缓存
```

实例如下：

```
open_log_file_cache max=1000 inactive=20s valid=1m min_uses=2;
open_log_file_cache max=1000 inactive=20s valid=1m min_uses=2;
```



#### log_not_found指令

```
语法: log_not_found on | off;
默认值: log_not_found on;
配置段: http, server, location
是否在error_log中记录不存在的错误。默认是。
```



#### log_subrequest指令

```
语法: log_subrequest on | off;
默认值: log_subrequest off;
配置段: http, server, location
是否在access_log中记录子请求的访问日志。默认不记录。
```



#### rewrite_log指令

```
由ngx_http_rewrite_module模块提供的。用来记录重写日志的。对于调试重写规则建议开启。 Nginx重写规则指南
语法: rewrite_log on | off;
默认值: rewrite_log off;
配置段: http, server, location, if
启用时将在error log中记录notice级别的重写日志。
```



#### error_log指令

```
语法: error_log file | stderr | syslog:server=address[,parameter=value] [debug | info | notice | warn | error | crit | alert | emerg];
默认值: error_log logs/error.log error;
配置段: main, http, server, location
配置错误日志。
```



### [日志切割](https://www.nginx.com/resources/wiki/start/topics/examples/logrotation/)

> nginx日志默认情况下统统写入到一个文件中，文件会变的越来越大，非常不方便查看分析。以日期来作为日志的切割是比较好的，通常我们是以每日来做统计的。下面来说说nginx日志切割。



#### 定义日志轮滚策略
`/etc/logrotate.d/nginx`

```
/var/log/nginx/*.log {
        daily
        missingok
        rotate 52
        compress
        delaycompress
        notifempty
        create 640 nginx adm
        sharedscripts
        postrotate
                if [ -f /var/run/nginx.pid ]; then
                        kill -USR1 `cat /var/run/nginx.pid`
                fi
        endscript
}
```



手动执行，测试一下。

```
/usr/sbin/logrotate -f /etc/logrotate.d/nginx
```

> /var/log/nginx/*.log 使用通配符时，/var/log/nginx/ 目录下的所有匹配到的日志文件都将切割。如果要切割特定日志文件，就指定到文件名。



#### 设置计划任务

`/etc/crontab`

```
59 23 * * * root ( /usr/sbin/logrotate -f /PATH/TO/nginx-log-rotate)
```

> 每天23点59分钟执行日志切割。



## 虚拟主机

### 新建目录
```
#站点目录
mkdir /data/wwwroot -p
cd /data/wwwroot/
mkdir a.ttlsa.com
mkdir b.ttlsa.com
#日志文件目录
mkdir /data/logs/nginx -p
```

#### 配置nginx主配置文件
```
vim /usr/local/nginx/conf/nginx.conf
server{
server_name a.ttlsa.com;
listen 80;
root /data/wwwroot/a.ttlsa.com;
 
access_log /data/logs/nginx/a.ttlsa.com-access.log main;
location /
{
 
}
}
 
server{
server_name b.ttlsa.com;
listen 80;
root /data/wwwroot/b.ttlsa.com;
 
access_log /data/logs/nginx/b.ttlsa.com-access.log main;
location /
{
 
}
}
```

配置讲解

```
server{}：配置虚拟主机必须有这个段。
server_name：虚拟主机的域名，可以写多个域名，类似于别名，比如说你可以配置成
server_name b.ttlsa.com c.ttlsa.com d.ttlsa.com，这样的话，访问任何一个域名，内容都是一样的
listen 80，监听ip和端口，这边仅仅只有端口，表示当前服务器所有ip的80端口，如果只想监听127.0.0.1的80，写法如下：
listen 127.0.0.1:80
root /data/site/b.ttlsa.com：站点根目录，你网站文件存放的地方。注：站点目录和域名尽量一样，养成一个好习惯
access_log /data/logs/nginx/b.ttlsa.com-access.log main：访问日志
location /{} 默认uri,location具体内容后续讲解,大家关注一下.
```

检查nginx配置文件并启动

```
/usr/local/nginx/sbin/nginx -t
/usr/local/nginx/sbin/nginx
```



## nginx正向代理

我们平时用的最多的最常见的是反向代理。反向代理想必都会配置的， 那么nginx的正向代理是如何配置的呢？

```
server {
	listen 8090;
	location / {
			resolver 218.85.157.99 218.85.152.99;
			resolver_timeout 30s;
			proxy_pass http://$host$request_uri;
	}
	access_log  /data/httplogs/proxy-$host-aceess.log;      
}
```

就这么简单哈。
测试：
http://www.ttlsa.com:8090

resolver指令

```
语法: resolver address ... [valid=time];
默认值: —
配置段: http, server, location
配置DNS服务器IP地址。可以指定多个，以轮询方式请求。
nginx会缓存解析的结果。默认情况下，缓存时间是名字解析响应中的TTL字段的值，可以通过valid参数更改。
```

resolver_timeout指令

```
语法: resolver_timeout time;
默认值: resolver_timeout 30s;
配置段: http, server, location
解析超时时间。
```



## nginx 反向代理

由于公司内网有多台服务器的http服务要映射到公司外网静态IP，如果用路由的端口映射来做，就只能一台内网服务器的80端口映射到外网80端口，其他服务器的80端口只能映射到外网的非80端口。非80端口的映射在访问的时候要域名加上端口，比较麻烦。并且公司入口路由最多只能做20个端口映射。肯定以后不够用。
然后k兄就提议可以在内网搭建个nginx反向代理服务器，将nginx反向代理服务器的80映射到外网IP的80，这样指向到公司外网IP的域名的HTTP请求就会发送到nginx反向代理服务器，利用nginx反向代理将不同域名的请求转发给内网不同机器的端口，就起到了“根据域名自动转发到相应服务器的特定端口”的效果，而路由器的端口映射做到的只是“根据不同端口自动转发到相应服务器的特定端口”，真是喜大普奔啊。
涉及的知识：nginx编译安装，nginx反向代理基本配置，路由端口映射知识，还有网络域名等常识。
本次实验目标是做到：在浏览器中输入xxx123.tk能访问到内网机器192.168.10.38的3000端口，输入xxx456.tk能访问到内网机器192.168.10.40的80端口。



```
client_max_body_size 50m; #缓冲区代理缓冲用户端请求的最大字节数,可以理解为保存到本地再传给用户
proxy_connect_timeout 300s; #nginx跟后端服务器连接超时时间(代理连接超时)
proxy_read_timeout 300s; #连接成功后，后端服务器响应时间(代理接收超时)
proxy_send_timeout 300s;
proxy_buffer_size 64k; #设置代理服务器（nginx）保存用户头信息的缓冲区大小
proxy_buffers 4 32k; #proxy_buffers缓冲区，网页平均在32k以下的话，这样设置
proxy_busy_buffers_size 64k; #高负荷下缓冲大小（proxy_buffers*2）
proxy_temp_file_write_size 64k; #设定缓存文件夹大小，大于这个值，将从upstream服务器传递请求，而不缓冲到磁盘
proxy_ignore_client_abort on; #不允许代理端主动关闭连接
```



编辑反向代理服务器配置文件：
vim /usr/local/nginx/conf/reverse-proxy.conf

```
server
{
    listen 80;
    server_name xxx123.tk;
    location / {
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://192.168.10.38:3000;
    }
    access_log logs/xxx123.tk_access.log;
}
 
server
{
    listen 80;
    server_name xxx456.tk;
    location / {
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://192.168.10.40:80;
    }
    access_log logs/xxx456.tk_access.log;
}
```



如果想对后端机器做负载均衡，像下面这配置就可以把对nagios.xxx123.tk的请求分发给内网的131和132这两台机器做负载均衡了。

```
upstream monitor_server {
    server 192.168.0.131:80;
    server 192.168.0.132:80;
}
 
server
{
    listen 80;
    server_name nagios.xxx123.tk;
    location / {
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
        proxy_pass http://monitor_server;
    }
    access_log logs/nagios.xxx123.tk_access.log;
}
```

额，关于负载均衡和缓存就不多说了，这里只是要起到一个简单的“域名转发”功能。
另外，由于http请求最后都是由反向代理服务器传递给后段的机器，所以后端的机器原来的访问日志记录的访问IP都是反向代理服务器的IP。
要想能记录真实IP，需要修改后端机器的日志格式，这里假设后端也是一台nginx：
在后端配置文件里面加入这一段即可：

```
log_format access '$HTTP_X_REAL_IP - $remote_user [$time_local] "$request" '
'$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" $HTTP_X_Forwarded_For';
 
access_log logs/access.log access;
```

再看看原来日志的格式长什么样：

```
#log_format main '$remote_addr - $remote_user [$time_local] "$request" '
# '$status $body_bytes_sent "$http_referer" '
# '"$http_user_agent" "$http_x_forwarded_for"';
 
#access_log logs/access.log main;
```
之前没配置下面这段，访问时候偶尔会出现504 gateway timeout，由于偶尔出现，所以不太好排查。

从日志看来是连接超时了，网上一通乱查之后估计可能是后端服务器响应超时了，本着大胆假设，小心求证的原则，既然假设了错误原因就要做实验重现错误：那就调整代理超时参数，反过来把代理超时阀值设小（比如1ms）看会不会次次出现504。后来发现把proxy_read_timeout 这个参数设置成1ms的时候，每次访问都出现504。于是把这个参数调大，加入下面那段配置，解决问题了。

```
    proxy_connect_timeout 300s;
    proxy_read_timeout 300s;
    proxy_send_timeout 300s;
    proxy_buffer_size 64k;
    proxy_buffers 4 32k;
    proxy_busy_buffers_size 64k;
    proxy_temp_file_write_size 64k;
    proxy_ignore_client_abort on;
```



### TCP 代理

[nginx_tcp_proxy_module模块](http://yaoweibin.github.io/nginx_tcp_proxy_module/README.html)
nginx tcp代理功能由nginx_tcp_proxy_module模块提供，同时监测后端主机状态。该模块包括的模块有： ngx_tcp_module, ngx_tcp_core_module, ngx_tcp_upstream_module, ngx_tcp_proxy_module, ngx_tcp_upstream_ip_hash_module。



安装

```
wget http://nginx.org/download/nginx-1.4.4.tar.gz
tar zxvf nginx-1.4.4.tar.gz
cd nginx-1.4.4
./configure --add-module=/path/to/nginx_tcp_proxy_module
make
make install
```



配置

```
http {
    listen 80;
    location /status {
        check_status;
    }
}
tcp {
    upstream cluster_www_ttlsa_com {
        # simple round-robin
        server 127.0.0.1:1234;
        check interval=3000 rise=2 fall=5 timeout=1000;
        #check interval=3000 rise=2 fall=5 timeout=1000 type=ssl_hello;
        #check interval=3000 rise=2 fall=5 timeout=1000 type=http;
        #check_http_send "GET / HTTP/1.0\r\n\r\n";
        #check_http_expect_alive http_2xx http_3xx;
    }
    server {
        listen 8888;
        proxy_pass cluster_www_ttlsa_com;
    }
}
```

这会出现一个问题，就是tcp连接会掉线。原因在于当服务端关闭连接的时候，客户端不可能立刻发觉连接已经被关闭，需要等到当Nginx在执行check规则时认为服务端链接关闭，此时nginx会关闭与客户端的连接。



#### 保持连接配置

```
http {
    listen 80;
    location /status {
        check_status;
    }
}
tcp {
 timeout 1d;
    proxy_read_timeout 10d;
    proxy_send_timeout 10d;
    proxy_connect_timeout 30;
    upstream cluster_www_ttlsa_com {
        # simple round-robin
        server 127.0.0.1:1234;
        check interval=3000 rise=2 fall=5 timeout=1000;
        #check interval=3000 rise=2 fall=5 timeout=1000 type=ssl_hello;
        #check interval=3000 rise=2 fall=5 timeout=1000 type=http;
        #check_http_send "GET / HTTP/1.0\r\n\r\n";
        #check_http_expect_alive http_2xx http_3xx;
    }
    server {
        listen 8888;
        proxy_pass cluster_www_ttlsa_com;
 so_keepalive on;
        tcp_nodelay on;
    }
}
```



### 高可用nginx群集和高速缓存

nginx+keepalived+proxy_cache 配置高可用nginx群集和高速缓存

#### 安装nginx 主从服务器

下载

```
wget http://nginx.org/download/nginx-1.0.11.tar.gz
wget http://labs.frickle.com/files/ngx_cache_purge-1.4.tar.gz
```

安装

```
yum -y install zlib-devel pcre-devel openssl-devel  # 安装依赖
tar –xvf ngx_cache_purge-1.4.tar.gz
tar –xvf nginx-1.0.11.tar.gz
cd nginx-1.0.11/

./configure --prefix=/usr/local/nginx --add-module=../ngx_cache_purge-1.4 --with-http_stub_status_module --with-http_ssl_module --with-http_flv_module --with-http_gzip_static_module

make && make install
```



#### 配置

```
vi /usr/local/nginx/conf/nginx.conf

user nobody;
worker_processes 8;
events {
worker_connections 1024;
}
http {
include mime.types;
default_type application/octet-stream;
charset utf-8;
server_names_hash_bucket_size 128;
client_header_buffer_size 32k;
large_client_header_buffers 4 32k;
client_max_body_size 300m;
tcp_nopush on;
tcp_nodelay on;
client_body_buffer_size 512k;
proxy_connect_timeout 5;
proxy_read_timeout 60;
proxy_send_timeout 5;
proxy_buffer_size 16k;
proxy_buffers 4 64k;
proxy_busy_buffers_size 128k;
proxy_temp_file_write_size 128k;
sendfile on;
gzip on;
gzip_min_length 1k;
gzip_buffers 4 16k;
gzip_http_version 1.1;
gzip_comp_level 2;
gzip_types text/plain application/x-javascript text/css application/xml;
gzip_vary on;
proxy_temp_path /data/proxy_temp_dir;
proxy_cache_path /data/proxy_cache_dir levels=1:2 keys_zone=cache_one:50m inactive=1m max_size=2g;
upstream real_server_pool{
server 192.168.200.148:80 weight=1 max_fails=2 fail_timeout=30s;
server 192.168.10.251:80 weight=1 max_fails=2 fail_timeout=30s;
}
keepalive_timeout 65;
server {
listen 80;
server_name localhost;
location / {
root html;
index index.html index.htm;
proxy_next_upstream http_502 http_504 error timeout invalid_header;
proxy_cache cache_one;
proxy_cache_valid 200 304 12h;
proxy_cache_key $host$uri$is_args$args;
proxy_set_header Host $host;
proxy_set_header X-Forwarded-For $remote_addr;
proxy_pass http://real_server_pool;
expires 1d;
}
error_page 500 502 503 504 /50x.html;
location = /50x.html {
root html;
}

location ~ .*\.(php|jsp|cgi)?$
{
proxy_set_header Host $host;
proxy_set_header X-Forwarded-For $remote_addr;
proxy_pass http://real_server_pool;
}
log_format access '$remote_addr - $remote_user [$time_local] "$request"'
'$status $body_bytes_sent "$http_referer" '
'"$http_user_agent" $http_x_forwarded_for';
access_log /data/logs/access.log access;
}
}
```
备用调度器的nginx配置文件和主调度器的配置文件一样。



启动nginx

```
/usr/local/nginx/sbin/nginx
```



#### 安装keepalived（在nginx的mater和backup都安装）

下载

```
wget http://www.keepalived.org/software/keepalived-1.1.19.tar.gz
```



安装

```
tar zxvf keepalived-1.1.19.tar.gz
cd keepalived-1.1.19

./configure --prefix=/usr/local/keepalived

make
make install

cp /usr/local/keepalived/sbin/keepalived /usr/sbin/
cp /usr/local/keepalived/etc/sysconfig/keepalived /etc/sysconfig/
cp /usr/local/keepalived/etc/rc.d/init.d/keepalived /etc/init.d/
mkdir /etc/keepalived
```



#### 配置

```
vi /etc/keepalived/keepalived.conf

vrrp_instance VI_INET1 {
state MASTER
interface eth0
virtual_router_id 53
priority 100
advert_int 1
authentication {
auth_type pass
auth_pass 1111
}
virtual_ipaddress {
192.168.10.104/24
}
}
virtual_server 192.168.10.104 80 {
delay_loop 6
lb_algo rr
lb_kind NAT
nat_mask 255.255.255.0
persistence_timeout 50
protocol TCP
real_server 192.168.10.251 80 {
weight 3
TCP_CHECK {
connect_timeout 10
nb_get_retry 3
delay_before_retry 3
connect_port 80
}
}
real_server 192.168.200.148 80 {
weight 3
TCP_CHECK {
connect_timeout 10
nb_get_retry 3
delay_before_retry 3
connect_port 80
}
}
}
```

配置备用调度器的keepalived,只需要将state MASTER 改为state BACKUP,降低priority 100 的值:

```
state MASTER ---> state BACKUP
priority 100 --->  priority 99 （此值必须低于主的）
```



主备启动

```
/etc/init.d/keepalived start
```



## 优化指南

### 基本的 (优化过的)配置
我们将修改的唯一文件是nginx.conf，其中包含Nginx不同模块的所有设置。你应该能够在服务器的/etc/nginx目录中找到nginx.conf。首先，我们将谈论一些全局设置，然后按文件中的模块挨个来，谈一下哪些设置能够让你在大量客户端访问时拥有良好的性能，为什么它们会提高性能。本文的结尾有一个完整的配置文件。



#### 高层的配置

nginx.conf文件中，Nginx中有少数的几个高级配置在模块部分之上。

```
user www-data;
pid /var/run/nginx.pid;
worker_processes auto;
worker_rlimit_nofile 100000;
```
```
user和pid应该按默认设置 - 我们不会更改这些内容，因为更改与否没有什么不同。
worker_processes 定义了nginx对外提供web服务时的worder进程数。最优值取决于许多因素，包括（但不限于）CPU核的数量、存储数据的硬盘数量及负载模式。不能确定的时候，将其设置为可用的CPU内核数将是一个好的开始（设置为“auto”将尝试自动检测它）。
worker_rlimit_nofile 更改worker进程的最大打开文件数限制。如果没设置的话，这个值为操作系统的限制。设置后你的操作系统和Nginx可以处理比“ulimit -a”更多的文件，所以把这个值设高，这样nginx就不会有“too many open files”问题了。
```

#### Events模块
events模块中包含nginx中所有处理连接的设置。

```
events {
worker_connections 2048;
multi_accept on;
use epoll;
}
```
```
worker_connections设置可由一个worker进程同时打开的最大连接数。如果设置了上面提到的worker_rlimit_nofile，我们可以将这个值设得很高。
记住，最大客户数也由系统的可用socket连接数限制（~ 64K），所以设置不切实际的高没什么好处。
multi_accept 告诉nginx收到一个新连接通知后接受尽可能多的连接。
use 设置用于复用客户端线程的轮询方法。如果你使用Linux 2.6+，你应该使用epoll。如果你使用*BSD，你应该使用kqueue。想知道更多有关事件轮询？看下维基百科吧（注意，想了解一切的话可能需要neckbeard和操作系统的课程基础）
（值得注意的是如果你不知道Nginx该使用哪种轮询方法的话，它会选择一个最适合你操作系统的）
```

#### HTTP 模块
HTTP模块控制着nginx http处理的所有核心特性。因为这里只有很少的配置，所以我们只节选配置的一小部分。所有这些设置都应该在http模块中，甚至你不会特别的注意到这段设置。

```
http {
server_tokens off;
sendfile on;
tcp_nopush on;
tcp_nodelay on;
...
}
```
```
server_tokens 并不会让nginx执行的速度更快，但它可以关闭在错误页面中的nginx版本数字，这样对于安全性是有好处的。sendfile可以让sendfile()发挥作用。sendfile()可以在磁盘和TCP socket之间互相拷贝数据(或任意两个文件描述符)。Pre-sendfile是传送数据之前在用户空间申请数据缓冲区。之后用read()将数据从文件拷贝到这个缓冲区，write()将缓冲区数据写入网络。sendfile()是立即将数据从磁盘读到OS缓存。因为这种拷贝是在内核完成的，sendfile()要比组合read()和write()以及打开关闭丢弃缓冲更加有效(更多有关于sendfile)
tcp_nopush 告诉nginx在一个数据包里发送所有头文件，而不一个接一个的发送
tcp_nodelay 告诉nginx不要缓存数据，而是一段一段的发送--当需要及时发送数据时，就应该给应用设置这个属性，这样发送一小块数据信息时就不能立即得到返回值。
```

```
access_log off;
error_log /var/log/nginx/error.log crit;
```
```
access_log设置nginx是否将存储访问日志。关闭这个选项可以让读取磁盘IO操作更快(aka,YOLO)
error_log 告诉nginx只能记录严重的错误。
```

```
keepalive_timeout 10;
client_header_timeout 10;
client_body_timeout 10;
reset_timedout_connection on;
send_timeout 10;
```
```
keepalive_timeout 给客户端分配keep-alive链接超时时间。服务器将在这个超时时间过后关闭链接。我们将它设置低些可以让ngnix持续工作的时间更长。client_header_timeout 和client_body_timeout 设置请求头和请求体(各自)的超时时间。我们也可以把这个设置低些。
reset_timeout_connection告诉nginx关闭不响应的客户端连接。这将会释放那个客户端所占有的内存空间。
send_timeout 指定客户端的响应超时时间。这个设置不会用于整个转发器，而是在两次客户端读取操作之间。如果在这段时间内，客户端没有读取任何数据，nginx就会关闭连接。
```

```
limit_conn_zone $binary_remote_addr zone=addr:5m;
limit_conn addr 100;
```
```
limit_conn_zone设置用于保存各种key（比如当前连接数）的共享内存的参数。5m就是5兆字节，这个值应该被设置的足够大以存储（32K*5）32byte状态或者（16K*5）64byte状态。
limit_conn为给定的key设置最大连接数。这里key是addr，我们设置的值是100，也就是说我们允许每一个IP地址最多同时打开有100个连接。
```

```
include /etc/nginx/mime.types;
default_type text/html;
charset UTF-8;
```
```
include只是一个在当前文件中包含另一个文件内容的指令。这里我们使用它来加载稍后会用到的一系列的MIME类型。default_type设置文件使用的默认的MIME-type。
charset设置我们的头文件中的默认的字符集
以下两点对于性能的提升在伟大的WebMasters StackExchange中有解释。
```

```
gzip on;
gzip_disable "msie6";
# gzip_static on;
gzip_proxied any;
gzip_min_length 1000;
gzip_comp_level 4;
gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;
```
```
gzip是告诉nginx采用gzip压缩的形式发送数据。这将会减少我们发送的数据量。
gzip_disable为指定的客户端禁用gzip功能。我们设置成IE6或者更低版本以使我们的方案能够广泛兼容。
gzip_static告诉nginx在压缩资源之前，先查找是否有预先gzip处理过的资源。这要求你预先压缩你的文件（在这个例子中被注释掉了），从而允许你使用最高压缩比，这样nginx就不用再压缩这些文件了（想要更详尽的gzip_static的信息，请点击这里）。
gzip_proxied允许或者禁止压缩基于请求和响应的响应流。我们设置为any，意味着将会压缩所有的请求。
gzip_min_length设置对数据启用压缩的最少字节数。如果一个请求小于1000字节，我们最好不要压缩它，因为压缩这些小的数据会降低处理此请求的所有进程的速度。
gzip_comp_level设置数据的压缩等级。这个等级可以是1-9之间的任意数值，9是最慢但是压缩比最大的。我们设置为4，这是一个比较折中的设置。
gzip_type设置需要压缩的数据格式。上面例子中已经有一些了，你也可以再添加更多的格式。
```

```
open_file_cache max=100000 inactive=20s;
open_file_cache_valid 30s;
open_file_cache_min_uses 2;
open_file_cache_errors on;

include /etc/nginx/conf.d/*.conf;
include /etc/nginx/sites-enabled/*;
```
```
open_file_cache打开缓存的同时也指定了缓存最大数目，以及缓存的时间。我们可以设置一个相对高的最大时间，这样我们可以在它们不活动超过20秒后清除掉。
open_file_cache_valid 在open_file_cache中指定检测正确信息的间隔时间。
open_file_cache_min_uses 定义了open_file_cache中指令参数不活动时间期间里最小的文件数。
open_file_cache_errors指定了当搜索一个文件时是否缓存错误信息，也包括再次给配置中添加文件。我们也包括了服务器模块，这些是在不同文件中定义的。如果你的服务器模块不在这些位置，你就得修改这一行来指定正确的位置。
```

一个完整的配置

```
cat nginx.conf

user www-data;
pid /var/run/nginx.pid;
worker_processes auto;
worker_rlimit_nofile 100000;
events {
 worker_connections 2048;
 multi_accept on;
 use epoll;
}
http {
  server_tokens off;
 sendfile on;
tcp_nopush on;
 tcp_nodelay on;
 access_log off;
 error_log /var/log/nginx/error.log crit;
 keepalive_timeout 10;
 client_header_timeout 10;
 client_body_timeout 10;
reset_timedout_connection on;
 send_timeout 10;
 limit_conn_zone $binary_remote_addr zone=addr:5m;
 limit_conn addr 100;
 include /etc/nginx/mime.types;
 default_type text/html;
charset UTF-8;
gzip on;
 gzip_disable "msie6";
gzip_proxied any;
gzip_min_length 1000;
gzip_comp_level 6;
 gzip_types text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript;
 open_file_cache max=100000 inactive=20s;
open_file_cache_valid 30s;
open_file_cache_min_uses 2;
open_file_cache_errors on;
 include /etc/nginx/conf.d/*.conf;
 include /etc/nginx/sites-enabled/*;
}
```




## 功能

### 配置 Basic Auth 登录认证

http_auth

安装 httpd-tools

```
yum install httpd-tools -y
```



创建授权用户和密码

```shell
# 第一种
printf "用户名:$(openssl passwd -crypt 密码)\n" >>/data/nginx/passwd.db

# 第二种
htpasswd -c -d /data/nginx/passwd.db 用户名
```

> 这个配置文件存放路径可以随意指定, 这里我指定的是`nginx`配置文件目录, 其中用户名是指允许登录的用户名, 这个可以自定义。



配置 Nginx

```nginx
server {
    listen       80;   
    server_name  _;
		# 第一种
    auth_basic   "登录认证";  
    auth_basic_user_file /data/nginx/passwd.db;
    
    # 第二种
    location / {
	     auth_basic "登录认证";
	     auth_basic_user_file /data/nginx/passwd.db; 
    }
}
```



使用

```shell
# 浏览器中使用
直接在浏览器中输入地址, 会弹出用户密码输入框, 输入即可访问

# 使用 wget
wget --http-user=用户名 --http-passwd=密码 http://ip

# 使用 curl
curl -u 用户名:密码 -O http://ip
```



## 参考

[nginx的location配置详解](https://blog.csdn.net/tjcyjd/article/details/50897959)

[Nginx location 配置踩坑过程分享](https://blog.coding.net/blog/tips-in-configuring-Nginx-location)

[Nginx 的  从零开始配置](https://segmentfault.com/a/1190000009651161)

[nginx用户认证配置（ Basic HTTP authentication）](http://www.ttlsa.com/nginx/nginx-basic-http-authentication/)