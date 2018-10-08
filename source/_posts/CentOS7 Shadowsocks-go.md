---
title: CentOS7 Shadowsocks-go
date: 2018-07-23 12:07
---

# CentOS7 Shadowsocks-go

```shell
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
```

```shell
 yum install -y yum-utils device-mapper-persistent-data lvm2
```

```shell
 yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

```shell
yum list docker-ce --showduplicates | sort -r

yum install docker-ce-18.06.1.ce
```

```shell
docker pull cloverzrg/shadowsocks-go
```

```
docker run -d -p 192.81.131.192:8373:8373 -e PASSWORD=meiyoumima -e METHOD=chacha20 --restart always --name shadowsocks-go cloverzrg/shadowsocks-go
```

## 参考

[shadowsocks-go-docker](https://github.com/cloverzrg/shadowsocks-go-docker)