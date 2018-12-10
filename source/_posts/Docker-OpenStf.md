---
title: Docker Deployment OpenStf
date: 2018-12-08 11:11:43
---

# Deployment OpenStf

### IP address

- proxy role: `10.8.8.128`

- database role: `10.8.8.128`

- app role: `10.8.8.128`
- provider1: `10.8.8.131`

## Proxy role

- [nginx configuration](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#nginx-configuration)

### Database role

The database role requires the following units, UNLESS you already have a working RethinkDB server/cluster running somewhere. In that case you simply will not have this role, and should point your [to that server instead.

- [rethinkdb.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#rethinkdbservice)

### App role

The app role can contain any of the following units. You may distribute them as you wish, as long as the [assumptions above](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#assumptions) hold. Some units may have more requirements, they will be listed where applicable.

- [rethinkdb-proxy-28015.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#rethinkdb-proxy-28015service) 

- [stf-app@.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-appservice)
- [stf-auth@.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-authservice)
- [stf-log-rethinkdb.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-log-rethinkdbservice)
- [stf-migrate.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-migrateservice)
- [stf-processor@.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-processorservice)
- [stf-reaper.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-reaperservice)
- [stf-storage-plugin-apk@.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-storage-plugin-apkservice)
- [stf-storage-plugin-image@.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-storage-plugin-imageservice)
- [stf-storage-temp@.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-storage-tempservice)
- [stf-triproxy-app.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-triproxy-appservice)
- [stf-triproxy-dev.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-triproxy-devservice)
- [stf-websocket@.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-websocketservice)
- [stf-api@.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-apiservice)

### Provider role

The provider role requires the following units, which must be together on a single or more hosts.

- [adbd.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#adbdservice)

- [stf-provider@.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#stf-providerservice)

  

# Proxy role配置

## 拉取镜像

```shell
docker pull nginx:1.15.7-alpine
```

## 启动nignx容器

```shell
cd /data/nginx/conf/ 

vim stf.域名.conf
```

```nginx
#daemon off
worker_processes 4;

events {
  worker_connections 1024;
}

http {
  upstream stf_app {
    server 10.8.8.128:3100 max_fails=0;
  }

  upstream stf_auth {
    server 10.8.8.128:3101 max_fails=0;
  }

  upstream stf_storage_apk {
    server 10.8.8.128:3102 max_fails=0;
  }

  upstream stf_storage_image {
    server 10.8.8.128:3103 max_fails=0;
  }

  upstream stf_storage {
    server 10.8.8.128:3104 max_fails=0;
  }

  upstream stf_websocket {
    server 10.8.8.128:3105 max_fails=0;
  }

  upstream stf_api {
    server 10.8.8.128:3106 max_fails=0;
  }

  types {
    application/javascript  js;
    image/gif               gif;
    image/jpeg              jpg;
    text/css                css;
    text/html               html;
  }

  map $http_upgrade $connection_upgrade {
    default  upgrade;
    ''       close;
  }

  server {
    listen 80;
    server_name stf.ovwane.com;
    keepalive_timeout 70; 
#    resolver 114.114.114.114 8.8.8.8 valid=300s;
#    resolver_timeout 10s;

    # Handle stf-provider@floor1.service
    location ~ "^/d/provider1/([^/]+)/(?<port>[0-9]{5})/$" {
      proxy_pass http://10.8.8.131:$port/;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $connection_upgrade;
      proxy_set_header X-Forwarded-For $remote_addr;
      proxy_set_header X-Real-IP $remote_addr;
    }

    # Handle stf-provider@floor2.service
    location ~ "^/d/provider2/([^/]+)/(?<port>[0-9]{5})/$" {
      proxy_pass http://10.8.8.132:$port/;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $connection_upgrade;
      proxy_set_header X-Forwarded-For $remote_addr;
      proxy_set_header X-Real-IP $remote_addr;
    }

    location /auth/ {
      proxy_pass http://stf_auth/auth/;
    }

    location /api/ {
      proxy_pass http://stf_api/api/;
    }

    location /s/image/ {
      proxy_pass http://stf_storage_image;
    }

    location /s/apk/ {
      proxy_pass http://stf_storage_apk;
    }

    location /s/ {
      client_max_body_size 1024m;
      client_body_buffer_size 128k;
      proxy_pass http://stf_storage;
    }

    location /socket.io/ {
      proxy_pass http://stf_websocket;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $connection_upgrade;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Real-IP $http_x_real_ip;
    }

    location / {
      proxy_pass http://stf_app;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Real-IP $http_x_real_ip;
    }
  }
}
```

**启动nginx**

```shell
docker run -d --name stf-nginx-1.15.7 --net host -v /data/stf/nginx/nginx.conf:/etc/nginx/nginx.conf:ro nginx:1.15.7-alpine
```



# Database role配置

## 拉取镜像

```shell
docker pull rethinkdb:2.3.6 
```

## 启动rethinkdb

```shell
# docker run -d --name stf-rethinkdb-2.3.6 -v /data/stf/rethinkdb:/data -e "AUTHKEY=RETHINKDBAUTHKEYANY" --net host rethinkdb:2.3.6 rethinkdb --bind all --cache-size 8192 --http-port 8090 --no-update-check

docker run -d --name stf-rethinkdb-2.3.6 -v /data/stf/rethinkdb:/data --net host rethinkdb:2.3.6 rethinkdb --bind all --cache-size 8192 --http-port 8090 --no-update-check
```



# App role配置

## 拉取镜像

```shell
for name in openstf/stf:v3.4.0 openstf/ambassador:latest; do
	docker pull $name
done
```

## 添加变量

```shell
STF_VERSION=3.4.0 \
STF_HOST=stf.域名 \
SFT_URL=https://$STF_HOST \
STF_IMAGE=openstf/stf:v$STF_VERSION \
STF_SECRET=RETHINKDBAUTHKEYANY
```

## 启动stf-migrate 初始化数据库表

```shell
docker run -d --name stf-migrate-$STF_VERSION --net host $STF_IMAGE stf migrate
```

## 启动stf-app

```shell
docker run -d --name stf-app-$STF_VERSION --net host -e "SECRET=${STF_SECRET}" $STF_IMAGE stf app --port 3100 --auth-url $STF_URL/auth/mock/ --websocket-url ws://$STF_HOST/
```

## 启动stf-auth

```shell
docker run -d --name stf-auth-$STF_VERSION --net host -e "SECRET=${STF_SECRET}" $STF_IMAGE stf auth-mock --port 3101 --app-url $STF_URL
```

> 启动 stf-app和stf-auth之后就可以登录了。

## 启动stf-websocket

```shell
docker run -d --name stf-websocket-$STF_VERSION --net host -e "SECRET=${STF_SECRET}" $STF_IMAGE stf websocket --port 3105 --storage-url $STF_URL --connect-sub tcp://$STF_HOST:7150 --connect-push tcp://$STF_HOST:7170
```

## 启动stf-api

```shell
docker run -d --name stf-api-$STF_VERSION --net host -e "SECRET=${STF_SECRET}" $STF_IMAGE stf api --port 3106 --connect-sub tcp://$STF_HOST:7150 --connect-push tcp://$STF_HOST:7170
```

## 启动stf-storage-plugin-apk

```shell
docker run -d --name stf-storage-plugin-apk-$STF_VERSION --net host $STF_IMAGE stf storage-plugin-apk --port 3102 --storage-url $SFT_URL
```

## 启动stf-storage-plugin-image

```shell
docker run -d --name stf-storage-plugin-image-$STF_VERSION --net host $STF_IMAGE stf storage-plugin-image --port 3103 --storage-url $SFT_URL
```

## 启动stf-storage-temp

```shell
docker run -d --name stf-storage-temp-$STF_VERSION --net host $STF_IMAGE stf storage-temp --port 3104 --save-dir /data
```

## 启动stf-triproxy-app

```shell
docker run -d --name stf-triproxy-app-$STF_VERSION --net host $STF_IMAGE stf triproxy app --bind-pub "tcp://*:7150" --bind-dealer "tcp://*:7160" --bind-pull "tcp://*:7170"
```

## 启动stf-processor

```shell
docker run -d --name stf-processor-$STF_VERSION --net host $STF_IMAGE stf processor stf-processer --connect-app-dealer tcp://$SFT_HOST:7160 --connect-dev-dealer tcp://$SFT_HOST:7260
```

## 启动stf-triproxy-dev

```shell
docker run -d --name stf-triproxy-dev-$STF_VERSION --net host $STF_IMAGE stf triproxy dev --bind-pub "tcp://*:7250" --bind-dealer "tcp://*:7260" --bind-pull "tcp://*:7270"
```

## 启动stf-reaper

```shell
docker run -d --name stf-reaper-$STF_VERSION --net host $STF_IMAGE stf reaper dev --connect-push tcp://$SFT_HOST:7270 --connect-sub tcp://$SFT_HOST:7150 --heartbeat-timeout 30000
```

## 启动stf-log-rethinkdb（可选安装）

```shell
docker run -d --name stf-log-rethinkdb-$STF_VERSION --net host $STF_IMAGE stf log-rethinkdb --connect-sub tcp://devside.$SFT_HOST:7150
```

> devside或者appside



# Provider role配置

## 拉取镜像

```shell
for name in sorccu/adb:latest openstf/stf:v3.4.0; do
	docker pull $name
done
```

> 每台provider都要启动adbd和stf-provider。

## 启动adbd

```shell
docker run -d --name adbd --privileged --net host -v /dev/bus/usb:/dev/bus/usb sorccu/adb:latest
```

## 启动stf-provider1

```shell
docker run -d --name stf-provider1-3.4.0 --net host openstf/stf:v3.4.0 stf provider --name "provider1" --connect-sub tcp://devside.stf.ovwane.com:7250 --connect-push tcp://devside.stf.ovwane.com:7270 --storage-url http://stf.ovwane.com --public-ip provider1.stf.ovwane.com --min-port=15000 --max-port=25000 --heartbeat-interval 20000 --screen-ws-url-pattern "ws://stf.ovwane.com/d/provider1/<%= serial %>/<%= publicPort %>/"
```

## 启动stf-provider2

```shell
docker run -d --name stf-provider2-3.4.0 --net host openstf/stf:v3.4.0 stf provider --name "provider2" --connect-sub tcp://devside.stf.ovwane.com:7250 --connect-push tcp://devside.stf.ovwane.com:7270 --storage-url http://stf.ovwane.com --public-ip provider2.stf.ovwane.com --min-port=15000 --max-port=25000 --heartbeat-interval 20000 --screen-ws-url-pattern "ws://stf.ovwane.com/d/provider2/<%= serial %>/<%= publicPort %>/"
```



## 参考

[ STF 折腾之路 最后换成 Docker 来安装](https://testerhome.com/topics/10406)

[STF 正式环境 docker 化集群部署](https://testerhome.com/topics/12755)

[STF docker 集群部署，树莓派做子节点,附带完整配置](https://testerhome.com/topics/15027)

[Mac 上用 docker 安装 openstf--一步一坑从入门到放弃](https://testerhome.com/topics/12489)

[OpenStf Deployment](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md)

[STF docker 集群部署，树莓派做子节点,附带完整配置](https://testerhome.com/topics/15027)

[STF 开发环境搭建与制作 docker 镜像过程](https://testerhome.com/topics/5206)

