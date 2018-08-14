---
title: Gitlab教程
date: 2018-07-24 14:19:32
tags:
---

**部署 GitLab**

获取 GitLab 镜像

```shell
docker pull gitlab/gitlab-ce:11.1.4-ce.0
```

查看本地镜像

```shell
docker images 
```

创建目录

```shell
mkdir -p /data/gitlab/{config,data,logs}
```

创建并运行容器

```shell
docker run --detach \
    --hostname git.xxx.com \
    --publish 443:443 --publish 80:80 --publish 22:22 \
    --name gitlab-11.1.4 \
    --restart always \
    --volume /data/gitlab/config:/etc/gitlab:Z \
    --volume /data/gitlab/logs:/var/log/gitlab:Z \
    --volume /data/gitlab/data:/var/opt/gitlab:Z \
    gitlab/gitlab-ce:11.1.4-ce.0 
```

查看运行状态

```shell
docker ps
netstat -ntulap | grep docker
```

访问 GitLab

- <http://git.ovwane.com/>
  - 如果没有域名，直接使用 IP 访问即可。

初始账户

```
用户: root
密码: 5iveL!fe
```

首次登陆需要修改密码。

**修改配置文件**

添加https, 需要导入证书

 ```shell
# 进入挂载配置目录
cd /data/gitlab/config
# 创建密钥文件夹, 并放入证书
mkdir ssl;cd ssl
# 移动证书，使用的是Let’s Encrypt证书
mv fullchain.cer  域名.key .
 ```

修改配置文件

```
vi /data/gitlab/config/gitlab.rb

###################################################
# 添加外部请求的域名(如果不支持https, 可以改成http)
external_url 'https://git.xxx.com'
# 修改gitlab对应的时区 
gitlab_rails['time_zone'] = 'Asia/Shanghai'
# 配置ssl参数
nginx['enable'] = true
nginx['redirect_http_to_https'] = false
nginx['redirect_http_to_https_port'] = 443
nginx['ssl_certificate'] = "/etc/gitlab/ssl/fullchain.cer"
nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/域名.key"
###################################################
```

重启服务

```shell
# 方法一: 重启容器(其中xxxxxx是容器id)
docker restart xxxxxx

# 方法二: 登陆容器, 重启配置
docker exec -it  xxxxxx bash   
gitlab-ctl reconfigure
```

### 安装gitlab-runner

```shell
docker run -d --name gitlab-runner-11.1.0 --restart always \
  -v /data/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:v11.1.0
```

参数说明：

- -d: 设置容器后台运行
- --name：容器名称
- -restart always：每次启动容器就重启 gitlab-runner
- -v: 共享目录挂载

安装好后，执行`$ docker ps `查看容器是否运行。

### 注册和初始化

编辑`vim /data/gitlab-runner/config/config.toml`，手动修改配置：

```toml
concurrent = 1
check_interval = 0

[[runners]]
  name = "runner1"
  url = "https://git.xxx.com"
  token = "e7b1109634c7082f3363fccbb9f165"
  executor = "docker"
  [runners.docker]
    tls_verify = false
    image = "ruby:2.1"
    privileged = false
    disable_cache = false
    volumes = ["/cache"]
    shm_size = 0
  [runners.cache] 
```

或者交互式注册runner信息

```shell
docker exec -it gitlab-runner gitlab-ci-multi-runner register
```

```shell
# gitlab-ci-multi-runner register
Running in system-mode.
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
# https://git.xxx.com
Please enter the gitlab-ci token for this runner:
# v7YsuyhYduqSyxJi8fmW #在gitlib中寻找runner token
Please enter the gitlab-ci description for this runner:
# alpine 3.8
Please enter the gitlab-ci tags for this runner (comma separated):
# qa
Registering runner... succeeded                     runner=v7YsuyhY
Please enter the executor: kubernetes, parallels, ssh, docker+machine, virtualbox, docker-ssh+machine, docker, docker-ssh, shell:
# docker
Please enter the default Docker image (e.g. ruby:2.1):
# alpine:3.8
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
```

**注册后使用 `gitlab-runner list` 查阅配置**

**gitlab-runner unregister 命令**

1. 通过 url 和 token 取消注册 `gitlab-runner unregister --url https://git.xxx.com/ --token t0k3n`
2. 通过name取消注册 `gitlab-runner unregister --name test-runner`
3. 删除所有注册runner `gitlab-runner unregister --all-runners`

## 参考

 [GitLab Installation](https://about.gitlab.com/installation/)

[使用Docker来搭建gitlab](http://blog.daocloud.io/using-docker-deploy-gitlab/)

[通过 docker 搭建自用的 gitlab 服务](https://www.jianshu.com/p/8d8e6b45a514)

[GitLab Docker images](https://docs.gitlab.com/omnibus/docker/)

[在 CentOS 7 上使用 Docker 部署安装 GitLab](https://my.oschina.net/u/1432614/blog/658568)

 [Docker+Gitlab https](https://medium.com/@CoderAFI/docker-gitlab-3fa06d6ec0b5)

 [利用docker搭建gitlab及持续集成模块](http://blog.fatedier.com/2016/04/05/install-gitlab-supporting-ci-with-docker/#%E5%90%AF%E5%8A%A8-gitlab)

 [Docker+Gitlab 配置](https://medium.com/@CoderAFI/docker-gitlab-3fa06d6ec0b5)

[Docker部署GitLab](https://www.centos.bz/2017/09/docker%e9%83%a8%e7%bd%b2gitlab/)

[Gitlab CI 自动部署 asp.net core web api 到Docker容器](https://www.cnblogs.com/jesse2013/p/gitlab-ci-netcoreapi-on-docker.html)

[【GITLAB】 服务配置可持续集成部署的项目案例 - 安装篇](https://segmentfault.com/a/1190000013362589)

[Registering Runners](https://docs.gitlab.com/runner/register/index.html)

[使用 GitLab-CI 来自动创建 Docker 镜像](http://www.ttlsa.com/docker/use-gitlab-ci-to-automatically-create-docker-images/)

[Gitlab CI 与 Docker 的配置与整合流程](https://wjqwsp.github.io/2016/11/23/Gitlab-CI-%E4%B8%8E-Docker-%E7%9A%84%E9%85%8D%E7%BD%AE%E4%B8%8E%E6%95%B4%E5%90%88%E6%B5%81%E7%A8%8B/)