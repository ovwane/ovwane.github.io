---
title: Jenkins安装部署
date: 2016-03-15 07:43:04
categories:
- 技术
- Linux
tags:
- CentOS
- Jenkins
---

# Jenkins

## [Docker Jenkins](https://hub.docker.com/r/jenkins/jenkins/)

```shell
docker pull jenkins/jenkins:2.235.5-lts-centos7
```

```shell
#jenkins启动用户id 1000
chown -R 1000:1000 ~/docker/jenkins
```

```shell
docker run -d --name jenkins -p 8080:8080 -p 50000:50000 -e "LANG=en_US.UTF-8" -e "JAVA_OPTS=-Duser.timezone=Asia/Shanghai -Dfile.encoding=UTF-8 -Dsun.jnu.encoding=UTF-8" -v ~/docker/jenkins:/var/jenkins_home  jenkins/jenkins:2.235.5-lts-centos7
```

> 设置默认用户名 https://github.com/foxylion/docker-jenkins/blob/master/docker-images/master/default-user.groovy



## Nginx 反向代理

> https://wiki.jenkins-ci.org/display/JENKINS/Jenkins+behind+an+NGinX+reverse+proxy

```nginx
upstream jenkins {
  server 127.0.0.1:8080 fail_timeout=0;
}
 
server {
  listen 80;
  server_name jenkins.domain.tld;
  return 301 https://$host$request_uri;
}
 
server {
  listen 443 ssl;
  server_name jenkins.domain.tld;
 
  ssl_certificate /etc/nginx/ssl/server.crt;
  ssl_certificate_key /etc/nginx/ssl/server.key;
 
  location / {
    proxy_set_header        Host $host:$server_port;
    proxy_set_header        X-Real-IP $remote_addr;
    proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header        X-Forwarded-Proto $scheme;
    proxy_redirect http:// https://;
    proxy_pass              http://jenkins;
    # Required for new HTTP-based CLI
    proxy_http_version 1.1;
    proxy_request_buffering off;
    proxy_buffering off; # Required for HTTP-based CLI to work over SSL
    # workaround for https://issues.jenkins-ci.org/browse/JENKINS-45651
    add_header 'X-SSH-Endpoint' 'jenkins.domain.tld:50022' always;
  }
}
```



## 插件

>  [Jenkins插件源使用国内镜像中心的最新方法_DevOps持续集成的博客-CSDN博客_jenkins 插件源](https://blog.csdn.net/weixin_40046357/article/details/104489497) 
>
> https://hub.docker.com/r/jenkinszh/jenkins-zh/dockerfile

### Folders

### Git

### Email Extension

### Credentials Binding

### SSH Build Agents

### Localization: Chinese (Simplified)



## 节点

### ssh-agent

> https://github.com/jenkinsci/ssh-slaves-plugin/blob/master/doc/CONFIGURE.md
>
> ssh-slave 镜像和  ssh-agent 镜像 dockerfile内容一致。底层镜像 adoptopenjdk/openjdk8:jdk8u262-b10-alpine
>
> https://hub.docker.com/r/jenkins/ssh-agent/dockerfile
>
> https://hub.docker.com/r/jenkins/ssh-slave/dockerfile

 

```bash
docker pull jenkins/ssh-agent:3.0.0
```



生成 key

```
ssh-keygen -f pemkey -m PEM -t ed25519
```



运行

```bash
docker run -d --name jenkins-ssh-agent jenkins/ssh-agent:3.0.0 "ssh 公钥"
```



## 问题

### 执行 shell 后不删除 jenkins 启动的子 shell

shell 添加  `BUILD_ID=DONTKILLME`



## 参考

[docker运行jenkins](https://www.jianshu.com/p/3671eb8de971)

[Create a HTTP proxy for jenkins using NGINX.](https://gist.github.com/rdegges/913102)

[docker容器的时区问题和中文问题](https://www.jianshu.com/p/9299e2685976)

[Jenkins](https://jenkins.io/download/)

[Installing Jenkins with Docker](https://wiki.jenkins.io/display/JENKINS/Installing+Jenkins+with+Docker)