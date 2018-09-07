---
title: Let's Encrypt 泛域名证书申请
date: 2018-09-02 00:37:47
tags:
---

```shell
acme.sh --issue --dns dns_he --keylength 4096 -d *.xxx.com -d xxx.com
```

nginx配置

```shell
ssl_certificate     fullchain.cer;
ssl_certificate_key xxx.com.key;
```

