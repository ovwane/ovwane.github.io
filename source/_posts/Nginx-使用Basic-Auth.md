---
title: Nginx 使用Basic Auth
date: 2018-12-08 12:47:23
tags:
---

# 配置Basic Auth登录认证

## 安装`httpd-tools` 

```shell
yum install httpd-tools -y
```

## 创建授权用户和密码

```shell
htpasswd -c -d /data/nginx/passwd.db 用户名
```

> 这个配置文件存放路径可以随意指定, 这里我指定的是`nginx`配置文件目录, 其中用户名是指允许登录的用户名, 这个可以自定义。

## 配置Nginx

大致配置如下:

```nginx
server {
    listen       80;   
    server_name  ;

    auth_basic   "登录认证";  
    auth_basic_user_file /data/nginx/passwd.db;
}
```

> 其中 `auth_basic` 和 `auth_basic_user_file` 是认证的配置, 注意密码文件的路径一定是上面生成的

## 使用

```shell
# 浏览器中使用
直接在浏览器中输入地址, 会弹出用户密码输入框, 输入即可访问

# 使用 wget
wget --http-user=用户名 --http-passwd=密码 http://ip

# 使用 curl
curl -u 用户名:密码 -O http://ip
```

## 参考

[Nginx配置Basic Auth登录认证](https://www.jianshu.com/p/b4a78af4e266)