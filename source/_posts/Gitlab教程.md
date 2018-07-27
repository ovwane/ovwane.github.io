---
title: Gitlab教程
date: 2018-07-24 14:19:32
tags:
---

#### 拉取gitlib镜像

```shell
docker pull gitlab/gitlab-ce:11.1.0-ce.0
```

#### 数据持久化保存

因为容器的数据是不能持久化保存的。所以我们需要用 docker volume 的方式将存储的数据映射到操作系统的目录中来。这样就算运行的容器崩溃，我们重新启动一个新的容器，原来容器中的数据还是不会丢失

我们建立了目录 /data/gitlab 来保存 gitlab 容器中的数据

```shell
mkdir -p  /data/gitlab
```

#### 运行 gitlab

```shell
docker run --detach \
    --hostname gitlib.example.com \
    --publish IP:443:443 --publish IP:80:80 --publish IP:22:22 \
    --name gitlab \
    --restart always \
    --volume /data/gitlab/config:/etc/gitlab \
    --volume /data/gitlab/logs:/var/log/gitlab \
    --volume /data/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce:11.1.0-ce.0
```

 这里把主机的 443、80、22 端口直接转发到容器，同时利用 --volume /data/gitlab/config:/etc/gitlab 、  --volume /data/gitlab/logs:/var/log/gitlab 、 --volume /data/gitlab/data:/var/opt/gitlab 这三个参数将 gitlab 的配置、数据和日志持久化到主机文件系统上来。需要绑定IP，不然由ipv6的只绑定ipv6地址。

## 参考

 [GitLab Installation](https://about.gitlab.com/installation/)

[通过 docker 搭建自用的 gitlab 服务](https://www.jianshu.com/p/8d8e6b45a514)

[GitLab Docker images](https://docs.gitlab.com/omnibus/docker/)



 

 

 