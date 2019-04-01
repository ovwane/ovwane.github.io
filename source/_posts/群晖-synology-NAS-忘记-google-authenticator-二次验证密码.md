---
title: 群晖 synology NAS 忘记 google authenticator 二次验证密码
date: 2019-03-26 07:33:25
tags:
---



## 登录22端口， 删除google_authenticator文件

> 开启了SSH，则登录22端口， 删除google_authenticator文件

首先通过SSH登录NAS

```shell
ssh user@ip
```



查找google Authenticator 参数文件。

```shell
sudo find /usr -name google_authenticator
```



将会搜索到以下路径的google_authenticator文件。

```
/usr/syno/etc/preference/用户名/google_authenticator
```



删除这个文件

```shell
rm /usr/syno/etc/preference/用户名/google_authenticator
```



## 参考

[群晖synology NAS ds 1815+忘记google authenticator二次验证密码](http://blog.newxd.com/7494.html)