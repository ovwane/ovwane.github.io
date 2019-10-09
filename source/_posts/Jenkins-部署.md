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

## Jenkins安装部署

下载 jenkins war包
下载tomcat
下载jdk

###配置jenkins安装环境
建立/usr/local/下的jdk软连接方便以后版本升级 ：

```
ln -s /usr/local/jdk1.8.0_151 /usr/local/jdk
ln -s /usr/local/apache-tomcat-8.5.23 /usr/local/tomcat
```


mkdir /data/webapps -p

/usr/local/tomcat/conf

### 启动tomcat

`./startup.sh`
server.xml

```
Connector port="80" 

<Host name="jenkins.ovwane.com"  appBase="/data/webapps"
149             unpackWARs="true" autoDeploy="true">
```



## [Docker Jenkins](https://hub.docker.com/r/jenkins/jenkins/)

```shell
docker pull jenkins/jenkins:2.138.2
```

```shell
#jenkins启动用户id 1000
chown -R 1000:1000 /data/jenkins
```

```shell
docker run --network=sonar_sonarnet -p 8810:8080 -p 50000:50000 --env JAVA_OPTS=-Duser.timezone=Asia/Shanghai -v ~/docker/jenkins:/var/jenkins_home -d --name jenkins-2.138.2 jenkins/jenkins:2.138.2 
```



### nginx反向代理

```nginx
listen               443 ssl;

location / {
        proxy_set_header Host $http_host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-Proto https;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_redirect http:// https://;

        add_header Pragma "no-cache";
        proxy_pass http://127.0.0.1:8080;
    }
```



### 插件

Git

Email Extension



## 参考

[docker运行jenkins](https://www.jianshu.com/p/3671eb8de971)

[Create a HTTP proxy for jenkins using NGINX.](https://gist.github.com/rdegges/913102)

[docker容器的时区问题和中文问题](https://www.jianshu.com/p/9299e2685976)

[Jenkins](https://jenkins.io/download/)

[Installing Jenkins with Docker](https://wiki.jenkins.io/display/JENKINS/Installing+Jenkins+with+Docker)