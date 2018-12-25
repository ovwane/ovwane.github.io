---
title: 安装 Docker Compose
date: 2018-12-08 08:21:06
tags:
---

## 安装docker-compose

**下载[docker-compose releases](https://github.com/docker/compose/releases)**

```shell
curl -L https://github.com/docker/compose/releases/download/1.23.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
```

**bash/zsh 补全命令**

```shell
curl -L https://raw.githubusercontent.com/docker/compose/1.23.2/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
```

**查看配置文件**

```shell
docker-compose config
```

## 参考

[制作nginx的dockerfile文件，使用docker-compose启动服务](https://www.jianshu.com/p/1656280a9c22)

[docker-compose.yml 变量使用](https://docs.docker.com/compose/compose-file/#variable-substitution)

[使用 Docker-Compose 编排容器](https://www.hi-linux.com/posts/12554.html)