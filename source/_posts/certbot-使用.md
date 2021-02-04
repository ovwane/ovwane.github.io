---
title: certbot 使用
date: 2020-07-29 13:32:50
tags:
---

# [certbot](https://certbot.eff.org/)



<!--more-->

```
pip install certbot certbot-nginx 
```

> Python 3



```
certbot --nginx
```



## 错误

### [UnicodeDecodeError: 'ascii' codec can't decode byte 0xe6 in position 2: ordinal not in range(128)](https://www.childsay.com/certbot-nginx-https-ascii-error.html)

nginx 配置文件有中文。删除掉就好。



## 参考

 [CentOS 7 配置nginx配置https_Lancelot的专栏-CSDN博客_pyopenssl ocsp](https://blog.csdn.net/qq_14952889/article/details/81985496) 

