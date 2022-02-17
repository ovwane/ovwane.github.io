---
title: WordPress
date: 2015-05-17 15:21:20
categories:
- 技术
- Linux
tags:
- CentOS
- Nginx
- httpd
- MySQL
- PHP
---

# [WordPress](https://wordpress.com/)



## LNAMP配置运行WordPress

## [nginx 301重定向配置](http://www.ttlsa.com/nginx/nginx-301-redirect/)
301重定向不陌生, 有时候有需求把某目录整个重定向到一个二级域名,或者不带www的顶级域名请求全部重定向到带www的二级域名.如果是Apache，需要配置.htaccess，nginx不支持，需要在配置文件里面使用rewrite指令来实现。

顶级域名重定向到www

```
server {
 server_name ttlsa.com;
 rewrite ^/(.*)$ http://www.ttlsa.com/$1 permanent;
 }
```
如上配置，所有ttlsa.com的请求都会重定向到www.ttlsa.com,301重定向对SEO很有帮助.这个配置大家用的最多。

www二级域名重定向到顶级域名

```
server {
 server_name www.ttlsa.com;
 rewrite ^/(.*)$ http://domain.com/$1 permanent;
 }
```

江湖盛传顶级域名的权重会比www二级域名的权重高，有些seoer会要求运维一定要把www的请求转到顶级域名，和上面的做法相反。

目录重定向

```
if ( $request_filename ~ nginxjiaocheng/ ) {
 rewrite ^ http://www.ttlsa.com/nginx/? permanent;
 }
```

目录跳转新域名

```
if ( $request_filename ~ nginx/ ) {
 rewrite ^ http://nginx.ttlsa.com/? permanent;
 }
```



## FAQ

### 更新不使用FTP

wp-config.php

```php
define('FS_METHOD','direct');
```

修改目录权限

```shell
chown -R www:www /wordpress(wp安装目录)
```

> www 是 web服务用户名



## 参考

 [WordPress 安装主题、插件、更新时需要FTP的解决办法_WPCOM](https://www.wpcom.cn/tutorial/101.html) 