---
title: Docker 运行Jenkins
date: 2018-09-02 00:01:28
tags:
---

```shell
docker pull jenkins/jenkins:2.138.2

#jenkins启动用户id 1000
chown -R 1000:1000 /data/jenkins

docker run --network=sonar_sonarnet -p 8810:8080 -p 50000:50000 --env JAVA_OPTS=-Duser.timezone=Asia/Shanghai -v ~/docker/jenkins:/var/jenkins_home -d --name jenkins-2.138.2 jenkins/jenkins:2.138.2 
```

nginx反向代理

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

**插件**

Git

Email Extension



## 参考

[docker运行jenkins](https://www.jianshu.com/p/3671eb8de971)

[Create a HTTP proxy for jenkins using NGINX.](https://gist.github.com/rdegges/913102)

[docker容器的时区问题和中文问题](https://www.jianshu.com/p/9299e2685976)