---
title: CentOS7 Shadowsocks-go
date: 2018-07-23 12:07
---

# CentOS7 Shadowsocks-go

```
docker pull cloverzrg/shadowsocks-go

docker run -d -p 192.81.131.192:8373:8373 -e PASSWORD=meiyoumima -e METHOD=chacha20 --restart always  cloverzrg/shadowsocks-go
```

## 参考

[shadowsocks-go-docker](https://github.com/cloverzrg/shadowsocks-go-docker)