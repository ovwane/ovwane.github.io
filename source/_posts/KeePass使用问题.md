---
date: 2018-08-08 20:31:20
---

KeePass使用问题

有些网站不能自动输入密码

例如 aliyun.com
它的密码输入框引用的是"https://passport.alibaba.com/"阿里巴巴的登录

KeePassXC
设置

```
KeePassHttp Settings

{"Allow":["passport.alibaba.com"],"Deny":[],"Realm":""}
```

允许多个域名

```
KeePassHttp Settings

{"Allow":["mailsso.mxhichina.com","passport.alibaba.com"],"Deny":[],"Realm":""}
```