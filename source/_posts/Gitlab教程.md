---
title: Gitlab教程
date: 2018-07-24 14:19:32
tags:
---

**部署 GitLab**

获取 GitLab 镜像

```shell
docker pull gitlab/gitlab-ce:8.0.5-ce.0
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
       --hostname git.quanjinlong.cn \
       --publish 443:443 \
       --publish 80:80 \
       --publish 22:22 \
       --name gitlab-8.0.5 \
       --restart always \
       --volume /data/gitlab/config:/etc/gitlab \
       --volume /data/gitlab/logs:/var/log/gitlab \
       --volume /data/gitlab/data:/var/opt/gitlab \
       gitlab/gitlab-ce:8.0.5-ce.0
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