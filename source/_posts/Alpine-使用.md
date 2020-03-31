---
title: Alpine 使用
date: 2020-07-14 13:38:44
tags:
---

# [Alpine Linux](https://alpinelinux.org/)



<!--more-->

```sh
docker run -d --name alpine -p 22:22 alpine:3.9 crond -f
```



源 [USTC](http://mirrors.ustc.edu.cn/help/alpine.html)

```
sed -i 's#http://dl-cdn.alpinelinux.org#https://mirrors.ustc.edu.cn#g' /etc/apk/repositories
```



设置时区 [Docker 镜像，基于 alpine 系统的时区配置_运维_偶尔记一下 - mybatis.io-CSDN博客](https://blog.csdn.net/isea533/article/details/87261764)

```
apk add tzdata && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone && date -R && apk del tzdata
```



定时任务 [在docker的alpine环境下使用crond，logrotate，syslogd来管理日志](https://weinan.io/2019/04/06/cron.html)

```
crond
```

> /etc/crontabs/root



## 服务

### SSHD

```sh
apk add openssh

if [ ! -f "/etc/ssh/ssh_host_rsa_key" ]; then
	# generate fresh rsa key
	ssh-keygen -f /etc/ssh/ssh_host_rsa_key -N '' -t rsa
fi
if [ ! -f "/etc/ssh/ssh_host_ecdsa_key" ]; then
	# generate fresh dsa key
	ssh-keygen -f /etc/ssh/ssh_host_ecdsa_key -N '' -t dsa
fi
if [ ! -f "/etc/ssh/ssh_host_ed25519_key" ]; then
	# generate fresh ed25519 key
	ssh-keygen -f /etc/ssh/ssh_host_ed25519_key -N '' -t ed25519
fi

passwd root

/usr/sbin/sshd -D &

PermitRootLogin yes

PasswordAuthentication yes
```

