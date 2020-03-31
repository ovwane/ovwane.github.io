---
title: Nginx 源码
date: 2020-04-22 11:20:03
tags:
---

# Nginx

[nginx documentation](http://nginx.org/en/docs/)



依赖环境：

- [PCRE](http://pcre.org/) – Supports regular expressions. Required by the NGINX [Core](https://nginx.org/en/docs/ngx_core_module.html) and [Rewrite](https://nginx.org/en/docs/http/ngx_http_rewrite_module.html) modules.
- [zlib](https://www.zlib.net/) – Supports header compression. Required by the NGINX [Gzip](https://nginx.org/en/docs/http/ngx_http_gzip_module.html) module.
- [OpenSSL](https://www.openssl.org/) – Supports the HTTPS protocol. Required by the NGINX [SSL](https://nginx.org/en/docs/http/ngx_http_ssl_module.html) module and others.



<!--more-->



## 安装

### 安装依赖

 [NGINX Docs | Installing NGINX Open Source](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/#compiling-and-installing-from-source) 



### 下载源码

 [Contributing Changes](http://nginx.org/en/docs/contributing_changes.html) 

http://nginx.org/download/nginx-1.18.0.tar.gz



安装 hg

```
brew install hg
```



下载代码

```shell
hg clone http://hg.nginx.org/nginx
```



### 编译参数

 [Building nginx from Sources](http://nginx.org/en/docs/configure.html) 

```shell

```



### 代码架构

 [Development guide](http://nginx.org/en/docs/dev/development_guide.html) 



## 参考

 [nginx documentation](http://nginx.org/en/docs/)

