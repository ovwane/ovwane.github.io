---
title: CentOS7 配置Shadowsocks-go
date: 2018-07-23 12:07:39
tags: CentOS
---

# CentOS7 Shadowsocks-go

获取ip

```shell
IP_ADDR=$(ip a|grep "inet "|awk -F\  '{print $2}'|awk '{print $1}'|grep -v '127.*'|grep -v '172.*'|awk -F/ '{print $1}')

echo $IP_ADDR
```

安装docker

```shell
yum -y install docker

systemctl enable docker

systemctl start docker
```

安装shadowsocks-go

```
docker pull cloverzrg/shadowsocks-go

docker run -d -p $IP_ADDR:8373:8373 -e PASSWORD=meiyoumima -e METHOD=chacha20 --restart always  cloverzrg/shadowsocks-go
```

## 参考

[shadowsocks-go-docker](https://github.com/cloverzrg/shadowsocks-go-docker)