---
title: Nginx配置
date: 2013-04-20 09:22:28
tags:
- Nginx
---

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

### 总结

Nginx 中的 location 并没有想象中的很难懂，不必害怕。多找资料看看，多尝试。你就会有收获。



# Nginx配置

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



### http_auth

配置nginx

```
location / {
	auth_basic "secret";
	auth_basic_user_file /data/nginx/passwd.db; 
}
```

生成密码

```shell
printf "admin:$(openssl passwd -crypt 123456)\n" >>/data/nginx/passwd.db
```



### 参考

[nginx的location配置详解](https://blog.csdn.net/tjcyjd/article/details/50897959)

[Nginx location 配置踩坑过程分享](https://blog.coding.net/blog/tips-in-configuring-Nginx-location)

[Nginx 的  从零开始配置](https://segmentfault.com/a/1190000009651161)



[nginx用户认证配置（ Basic HTTP authentication）](http://www.ttlsa.com/nginx/nginx-basic-http-authentication/)