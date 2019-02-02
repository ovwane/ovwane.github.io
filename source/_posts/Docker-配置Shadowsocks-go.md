---
title: Docker 配置Shadowsocks-go
date: 2018-07-23 12:07:39
tags: CentOS
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

启动 docker

```shell
systemctl enable docker

systemctl start docker
```

安装shadowsocks-go

```shell
docker pull cloverzrg/shadowsocks-go
```

获取ip

```shell
IP_ADDR=$(ip a|grep "inet "|awk -F\  '{print $2}'|awk '{print $1}'|grep -v '127.*'|grep -v '172.*'|awk -F/ '{print $1}')

echo $IP_ADDR
```

```shell
docker run -d --name shadowsocks-go -p $IP_ADDR:443:8373 -e PASSWORD=meiyoumima -e METHOD=chacha20 --restart always  cloverzrg/shadowsocks-go
```



## 参考

[shadowsocks-go-docker](https://github.com/cloverzrg/shadowsocks-go-docker)