---
title: Docker 配置
date: 2018-08-08 20:36:41
---

# Docker

## 安装

```bash
# step 1: 安装必要的一些系统工具
sudo yum install -y yum-utils device-mapper-persistent-data lvm2 firewalld
# Step 2: 添加软件源信息
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# Step 3: 更新并安装 Docker-CE
sudo yum makecache fast
sudo yum -y install docker-ce
# Step 4: 开启Docker服务
sudo service docker start

# 注意：
# Step 1: 查找Docker-CE的版本:
# yum list docker-ce.x86_64 --showduplicates | sort -r
#   Loading mirror speeds from cached hostfile
#   Loaded plugins: branch, fastestmirror, langpacks
#   docker-ce.x86_64            17.03.1.ce-1.el7.centos            docker-ce-stable
#   docker-ce.x86_64            17.03.1.ce-1.el7.centos            @docker-ce-stable
#   docker-ce.x86_64            17.03.0.ce-1.el7.centos            docker-ce-stable
#   Available Packages
# Step2 : 安装指定版本的Docker-CE: (VERSION 例如上面的 17.03.0.ce.1-1.el7.centos)
# sudo yum -y install docker-ce-[VERSION]
```

```shell
systemctl enable docker.service
systemctl start docker.service
systemctl status docker.service
```

## Docker proxy

> https://docs.docker.com/network/proxy/
>
> 可以加速 docker pull 命令下载镜像。

~/.docker/config.json

```json
{
 "proxies":
 {
   "default":
   {
     "httpProxy": "http://127.0.0.1:3001",
     "httpsProxy": "http://127.0.0.1:3001",
     "noProxy": "*.test.example.com,.example2.com,127.0.0.0/8"
   }
 }
}
```



### 配置镜像加速器

[Docker CE 镜像源站-阿里云](https://yq.aliyun.com/articles/110806)

/etc/docker/daemon.json

```
{
    "registry-mirrors": ["https://mqxz7mjm.mirror.aliyuncs.com"]
}
```

```shell
systemctl restart docker
```



### 安装docker-compose

下载 [docker-compose releases](https://github.com/docker/compose/releases)

```shell
curl -L https://github.com/docker/compose/releases/download/1.23.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
```



bash/zsh 补全命令

```shell
curl -L https://raw.githubusercontent.com/docker/compose/1.23.2/contrib/completion/bash/docker-compose > /etc/bash_completion.d/docker-compose
```



查看版本

```shell
docker-compose version
```



## 常用操作

下载一个标签下的所有镜像

```bash
docker image pull -a ubuntu
```

> 下载所有 ubuntu 镜像。



查看日志位置

```
docker inspect --format='{{.LogPath}}' 
```



查看正在运行的容器是通过什么命令启动的

```bash
docker ps -a --no-trunc
```



修改运行中容器的端口，需要重启 Docker 服务。一般不推荐这么做。 https://blog.csdn.net/weixin_46152207/article/details/113684674



## 收集性能数据

 [Collect Docker metrics with Prometheus | Docker Documentation](https://docs.docker.com/config/daemon/prometheus/) 

/etc/docker/daemon.json

```json
{
  "metrics-addr" : "127.0.0.1:9323",
  "experimental" : true
}
```



## macOS Docker-Deskop

 [Getting a Shell in the Docker for Mac Moby VM](https://gist.github.com/BretFisher/5e1a0c7bcca4c735e716abf62afad389) 

启动终端，不太好使，字符容易乱。

```shell
screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty
```



启动终端

```
docker run -it --rm --privileged --pid=host justincormack/nsenter1
```



## 参考

