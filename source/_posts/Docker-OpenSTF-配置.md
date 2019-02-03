---
title: Docker OpenSTF 配置
date: 2018-10-26 14:06:01
tags:
---

**安装OpenSTF**

- mac下brew安装
- mac下vagrant安装
- ubuntu下docker安装
- ubuntu下docker安装stf-app

## macOS下安装OpenSTF

**安装依赖**

```shell
brew install graphicsmagick zeromq protobuf yasm pkg-config
```

-  rethinkdb 使用docker安装

**安装**[OpenSTF](https://github.com/openstf/stf)

node v8.13.0

python 2.7.15

stf 3.4.0

```shell
npm install -g stf
```

```verilog
node-pre-gyp info check checked for "/Users/jinlong/.nvm/versions/node/v8.13.0/lib/node_modules/stf/node_modules/jpeg-turbo/lib/binding/node-v57-darwin-x64/jpegturbo.node" (not found)
node-pre-gyp http GET https://pre-gyp.s3.amazonaws.com/jpegturbo/v0.4.0/jpegturbo-v0.4.0-node-v57-darwin-x64.tar.gz
node-pre-gyp verb download using proxy url: "http://127.0.0.1:1087"
node-pre-gyp http 403 https://pre-gyp.s3.amazonaws.com/jpegturbo/v0.4.0/jpegturbo-v0.4.0-node-v57-darwin-x64.tar.gz
node-pre-gyp http 403 status code downloading tarball https://pre-gyp.s3.amazonaws.com/jpegturbo/v0.4.0/jpegturbo-v0.4.0-node-v57-darwin-x64.tar.gz (falling back to source compile with node-gyp)
node-pre-gyp verb command build [ 'rebuild' ]
    
    
npm WARN deprecated @slack/client@3.16.0: v3.x and lower are no longer supported. see migration guide: https://github.com/slackapi/node-slack-sdk/wiki/Migration-Guide-for-v4
npm WARN deprecated node-uuid@1.4.8: Use uuid module instead
npm WARN deprecated boom@2.10.1: This version is no longer maintained. Please upgrade to the latest version.
npm WARN deprecated cryptiles@2.0.5: This version is no longer maintained. Please upgrade to the latest version.
npm WARN deprecated hoek@2.16.3: This version is no longer maintained. Please upgrade to the latest version.
npm WARN deprecated ejs@0.8.8: Critical security bugs fixed in 2.5.5
    
    
  CXX(target) Release/obj.target/zmq/binding.o
In file included from ../binding.cc:29:
/usr/local/Cellar/zeromq/4.2.5/include/zmq_utils.h:42:32: warning: unknown warning group '-Werror',
      ignored [-Wunknown-warning-option]
#pragma GCC diagnostic ignored "-Werror"
                               ^
/usr/local/Cellar/zeromq/4.2.5/include/zmq_utils.h:45:9: warning: Warning: zmq_utils.h is
      deprecated. All its functionality is provided by zmq.h. [-W#pragma-messages]
#pragma message(                                                               \
        ^
../binding.cc:999:15: warning: '~MessageReference' has a non-throwing exception specification but
      can still throw [-Wexceptions]
              throw std::runtime_error(ErrorMessage());
              ^
../binding.cc:997:18: note: destructor has a implicit non-throwing exception specification
          inline ~MessageReference() {
                 ^
../binding.cc:1205:11: warning: '~OutgoingMessage' has a non-throwing exception specification but
      can still throw [-Wexceptions]
          throw std::runtime_error(ErrorMessage());
          ^
../binding.cc:1203:14: note: destructor has a implicit non-throwing exception specification
      inline ~OutgoingMessage() {
             ^
4 warnings generated.
  SOLINK_MODULE(target) Release/zmq.node
ld: warning: directory not found for option '-L/opt/local/lib'
```

**启动**

```shell
stf local
```

**STFService.apk 位置**

```
.nvm/versions/node/v8.11.3/lib/node_modules/stf/vendor/STFService/STFService.apk
```

[ERR/provider 87127 [*] Device worker "67e2906f" died with code 1](https://github.com/openstf/stf/issues/670)

**开发者设置**->打开USB调试（安全设置）允许通过USB调试修改权限或**模拟点击**

```shell
adb uninstall jp.co.cyberagent.stf
adb install STFService.apk
adb shell am start -n jp.co.cyberagent.stf/.IdentityActivity
adb shell am startservice -n jp.co.cyberagent.stf/.Service
```



## Ubuntu下使用Docker安装

### [配置docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04)

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```shell
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```

```shell
sudo apt update

apt-cache policy docker-ce
```

**安装docker**

```shell
sudo apt install docker-ce
```

**启动docker**

```shell
sudo systemctl status docker
```

### [配置openstf](https://hub.docker.com/r/openstf/stf/)

**拉取镜像**

[openstf](https://hub.docker.com/u/openstf/)/[stf](https://hub.docker.com/r/openstf/stf/tags/)

```shell
docker pull openstf/stf:v3.4.0
```

[openstf](https://hub.docker.com/u/openstf/)/[ambassador](https://hub.docker.com/r/openstf/ambassador/)

```shell
docker pull openstf/ambassador:latest
```

[rethinkdb](https://hub.docker.com/r/library/rethinkdb/tags/)

```shell
docker pull rethinkdb:2.3.6
```

[sorccu](https://hub.docker.com/u/sorccu/)/[adb](https://hub.docker.com/r/sorccu/adb/tags)

```shell
docker pull sorccu/adb:latest
```

[nginx](https://hub.docker.com/r/_/nginx/tags)

```shell
docker pull nginx:1.15.7-alpine
```

```shell
for name in openstf/stf:v3.4.0 openstf/ambassador:latest rethinkdb:2.3.6 sorccu/adb:latest nginx:1.15.7-alpine; do
	docker pull $name
done
```

[解决Error response from daemon: Get https://registry-1.docker.io/v2](https://blog.csdn.net/quanqxj/article/details/79479943)

#### 启动镜像

- 启动数据库

```shell
docker run -d --name rethinkdb-2.3.6 --net host -v /data/docker/rethinkdb:/data rethinkdb:2.3.6
```
```shell
docker stop rethinkdb-2.3.6
docker rm rethinkdb-2.3.6
```

- 启动adb service

```shell
  docker run -d --name adbd --privileged --net host -v /dev/bus/usb:/dev/bus/usb sorccu/adb:latest
```
```shell
  docker stop adbd
  docker rm adbd
  docker start adbd
```
- 启动stf 启动的时配置的IP地址为你服务器的ip

```shell
  docker run -d --name stf-3.4.0 --net host openstf/stf:v3.4.0 stf local --public-ip 10.8.8.118
```
```shell
  docker stop stf-3.4.0
  docker rm stf-3.4.0
  docker start stf-3.4.0
```

# [STF 正式环境 docker 化集群部署](https://testerhome.com/topics/12755)

在本地开发STF，需依次安装Node.js，ADB ，RethinkDB，GraphicsMagick，ZeroMQ ，Protocol Buffers，yasm， pkg-config环境，如果部署到生产环境，每增加一台机器节点，都安装这些环境，那将是一个非常痛苦的过程。STF官方也推荐使用docker来搭建STF环境，服务器只需要安装docker，然后依次拉取以下镜像： docker pull openstf/stf:latest docker pull sorccu/adb:latest  docker pull rethinkdb:latest  docker pull openstf/ambassador:latest  docker pull nginx:latest openstf/stf是stf的主镜像，sorccu/adb是adb工具，如果本地服务器已经有adb环境，可以不要此镜像，rethinkdb是stf数据库镜像，openstf/ambassador为网络代理工具，nginx是一个web服务器反向代理工具， stf依赖它将不同的url转发到不同模块上，没有nginx，生产环境中的stf是肯定不能正常工作的。拉取这些镜像以后，stf就可以直接在容器中运行了，不需要再安装stf的相关工具，openstf/stf镜像已经包含了所依赖的环境。 如果基于stf做了二次定制，要在服务器上进行docker部署，可不是一个stf local命令就开始使用了，需要以下步骤：

## 发布镜像

```shell
git clone https://github.com/openstf/stf.git
cd stf/docker
```

openstf/stf是STF官方push到docker官方仓库的镜像，如果进行了二次开发，理论上只需要替换该镜像即可。制作镜像首先需要DockerFile，在源码的根目录下，已经提供了现成的DockerFile，直接基于该File制作镜像，在DockerFile目录下，执行 `sudo docker build -t dystf/stf:latest .`其中dystf/stf为镜像名称，latest为镜像版本。网速好的情况下，10分钟内可以制作好，网速不好的情况下，半个小时以上甚至失败都是有可能的，这个时候需要不断地重试，执行完后，执行`sudo doker images`可查看刚制作好的镜像。如果新发布的镜像有问题，可以快速恢复到上个版本。每次修改代码以后，基于主服务器重新制定新的镜像，可将该镜像保存，然后可以分发到其他服务器上，其他服务器不需要再重新制定，方便快捷，`sudo docker save 镜像ID> dystf_image.tar`

 ## 集群化部署

 如果只有10-15个设备的数据量，只需要一台服务器节点，直接启动stf local命令就可以了。当然实际生产环境中，绝对提供不止十几台的数量，有可能达到上百台，上百台的话，至少需要 10台服务器节点，一台服务器节点一般10台手机。所以只有通过集群部署来解决该问题，而且能够达到快速扩展新节点目的。 STF包含多个独立运行的进程，这些进程之间通过ZeroMQ和Protocol Buffers通信，每个进程节点叫做“unit”，这些单元可以分别部署到不同的服务器上，通过nigix配置来转发，详情可以看官方文档介绍。在我们的远程真机部署环境中，选了一台性能较好的服务器作为主节点，该服务器不连任何设备，provider节点作为设备的提供agent，它将设备上报给主服务器，并展示。

### **provider服务器节点部署以下单元模块**： 

- adbd.service（adb环境） 
- stf-provider@.service 

> stf-provider的作用就是给主框架提供手机，provider可以运行在同一台机器，也可以运行在其他机器，但是，这台机器一定要可直接访问的，因为手机屏幕其实就是从这里出来的。在provider机器上一定要先由adb的环境，毕竟安卓手机要依赖adb调试。

### **添加新provider节点** 

添加新的设备，需要增加新的服务器节点，新的节点只需几分钟便可接入完成： 

- 第一步：安装docker环境，adb环境。 

- 第二部：导入镜像，在上面第一步中，将主服务器已经发布好的镜像dystf_image.tar scp 到该服务器上，然后导入该镜像： `sudo docker load < /Users/mtp/dystf_image.tar` 刚导入的镜像没有名字和版本，需要对其打标，dystf/stf:latest为打标的名字和版本 `sudo docker tag $(sudo docker images --filter "dangling=true" -q) dystf/stf:latest `

- 第三部：启动stf-provider，如上命令。 

- 第四部：配置nginx 在主服务器的nginx.conf文件下，添加刚启动的provider节点配置，添加：

  ```nginx
  location ~ "^/d/新节点启动名称,如上例的agentX/(/+)/(?[0-9]{5})/$" { 
      proxy_pass http://新节点IP2:$port/; proxy_http_version 1.1; 
      proxy_set_header Upgrade $http_upgrade; 
      proxy_set_header Connection $connection_upgrade; 
      proxy_set_header X-Forwarded-For $remote_addr; 
      proxy_set_header X-Real-IP $remote_addr; 
  } 
  ```

  > 配置完成后，重启ngnix，新节点所连接的设备即可上报给主服务器。



# STF Setup Examples using Vagrant and Docker

```shell
git clone https://github.com/openstf/setup-examples.git
cd stf-setup-examples
```

### Create Rethinkdb Cluster

Lets create rethinkdb cluster. Go to the `db` folder and run `vagrant up`. Yeah, thats it.

```shell
cd ./db; vagrant up
```



# Deployment OpenStf

### IP address

- app role、database role: `10.8.8.128`
- provider1: `10.8.8.131`

### Database role

The database role requires the following units, UNLESS you already have a working RethinkDB server/cluster running somewhere. In that case you simply will not have this role, and should point your [rethinkdb-proxy-28015.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#rethinkdb-proxy-28015service) to that server instead.

- [rethinkdb.service](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#rethinkdbservice)

### App role

The app role can contain any of the following units. You may distribute them as you wish, as long as the [assumptions above](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md#assumptions) hold. Some units may have more requirements, they will be listed where applicable.

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

  

# App role配置

## 启动nignx容器

```shell
mkdir /data/stf/nginx/ -p

cd /data/stf/nginx/ 

vim nginx.conf
```

```nginx
#daemon off;
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
    location ~ "^/d/floor1/([^/]+)/(?<port>[0-9]{5})/$" {
      proxy_pass http://10.8.8.131:$port/;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection $connection_upgrade;
      proxy_set_header X-Forwarded-For $remote_addr;
      proxy_set_header X-Real-IP $remote_addr;
    }

    # Handle stf-provider@floor2.service
    location ~ "^/d/floor2/([^/]+)/(?<port>[0-9]{5})/$" {
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

## 启动rethinkdb

```shell
docker run -d --name stf-rethinkdb-2.3.6 -v /data/stf/rethinkdb:/data -e "AUTHKEY=RETHINKDBAUTHKEYANY" --net host rethinkdb:2.3.6 rethinkdb --bind all --cache-size 8192 --http-port 8090 --no-update-check
```

## 启动stf-migrate 初始化数据库表

```shell
docker run -d --name stf-migrate-3.4.0 --net host openstf/stf:v3.4.0 stf migrate
```

## 启动stf-app

```shell
docker run -d --name stf-app-3.4.0 --net host -e "SECRET=RETHINKDBAUTHKEYANY" openstf/stf:v3.4.0 stf app --port 3100 --auth-url $STF_URL/auth/mock/ --websocket-url ws://stf.ovwane.com/
```

## 启动stf-auth

```shell
docker run -d --name stf-auth-3.4.0 --net host -e "SECRET=RETHINKDBAUTHKEYANY" openstf/stf:v3.4.0 stf auth-mock --port 3101 --app-url http://stf.ovwane.com/
```

> 启动 stf-app和stf-auth之后就可以登录了。

## 启动stf-websocket

```shell
docker run -d --name stf-websocket-3.4.0 --net host -e "SECRET=RETHINKDBAUTHKEYANY" openstf/stf:v3.4.0 stf websocket --port 3105 --storage-url http://stf.ovwane.com/ --connect-sub tcp://stf.ovwane.com:7150 --connect-push tcp://stf.ovwane.com:7170
```

## 启动stf-api

```shell
docker run -d --name stf-api-3.4.0 --net host -e "SECRET=RETHINKDBAUTHKEYANY" openstf/stf:v3.4.0 stf api --port 3106 --connect-sub tcp://stf.ovwane.com:7150 --connect-push tcp://stf.ovwane.com:7170
```

## 启动stf-storage-plugin-apk

```shell
docker run -d --name stf-storage-plugin-apk-3.4.0 --net host openstf/stf:v3.4.0 stf storage-plugin-apk --port 3102 --storage-url http://stf.ovwane.com/
```

## 启动stf-storage-plugin-image

```shell
docker run -d --name stf-storage-plugin-image-3.4.0 --net host openstf/stf:v3.4.0 stf storage-plugin-image --port 3103 --storage-url http://stf.ovwane.com/
```

## 启动stf-storage-temp

```shell
docker run -d --name stf-storage-temp-3.4.0 --net host openstf/stf:v3.4.0 stf storage-temp --port 3104 --save-dir /data
```

## 启动stf-triproxy-app

```shell
docker run -d --name stf-triproxy-app-3.4.0 --net host openstf/stf:v3.4.0 stf triproxy app --bind-pub "tcp://*:7150" --bind-dealer "tcp://*:7160" --bind-pull "tcp://*:7170"
```

## 启动stf-processor

```shell
docker run -d --name stf-processor-3.4.0 --net host openstf/stf:v3.4.0 stf processor stf-processer --connect-app-dealer tcp://stf.ovwane.com:7160 --connect-dev-dealer tcp://stf.ovwane.com:7260
```

## 启动stf-triproxy-dev

```shell
docker run -d --name stf-triproxy-dev-3.4.0 --net host openstf/stf:v3.4.0 stf triproxy dev --bind-pub "tcp://*:7250" --bind-dealer "tcp://*:7260" --bind-pull "tcp://*:7270"
```

## 启动stf-reaper

```shell
docker run -d --name stf-reaper-3.4.0 --net host openstf/stf:v3.4.0 stf reaper dev --connect-push tcp://stf.ovwane.com:7270 --connect-sub tcp://stf.ovwane.com:7150 --heartbeat-timeout 30000
```

## 启动stf-log-rethinkdb（可选安装）

```shell
docker run -d --name stf-log-rethinkdb-3.4.0 --net host openstf/stf:v3.4.0 stf log-rethinkdb --connect-sub tcp://devside.stf.ovwane.com:7150
```

> devside或者appside



# Provider role配置

> 每台provider都要启动adbd和stf-provider。

## 启动adbd

```shell
docker run -d --name adbd --privileged --net host -v /dev/bus/usb:/dev/bus/usb sorccu/adb:latest
```

## 启动stf-provider1

```shell
docker run -d --name stf-provider-3.4.0-1 --net host openstf/stf:v3.4.0 stf provider --name "provider-1" --connect-sub tcp://devside.stf.ovwane.com:7250 --connect-push tcp://devside.stf.ovwane.com:7270 --storage-url http://stf.ovwane.com --public-ip provider1.stf.ovwane.com --min-port=15000 --max-port=25000 --heartbeat-interval 20000 --screen-ws-url-pattern "ws://stf.ovwane.com/d/floor1/<%= serial %>/<%= publicPort %>/"
```

## 启动stf-provider2

```shell
docker run -d --name stf-provider-3.4.0-2 --net host openstf/stf:v3.4.0 stf provider --name "provider-2" --connect-sub tcp://devside.stf.ovwane.com:7250 --connect-push tcp://devside.stf.ovwane.com:7270 --storage-url http://stf.ovwane.com --public-ip provider2.stf.ovwane.com --min-port=15000 --max-port=25000 --heartbeat-interval 20000 --screen-ws-url-pattern "ws://stf.ovwane.com/d/floor2/<%= serial %>/<%= publicPort %>/"
```



## 错误

- [INF/provider 1 [*] Providing all 0 of 2 device(s); ignoring "67e2906f"](https://github.com/openstf/stf/issues/627)：原因是provider连不上stf-app。
- Lost device

```verilog
root@ubuntu:~# docker logs -f stf_provider_stf-provider_1
2018-12-12T12:52:15.648Z INF/provider 1 [*] Subscribing to permanent channel "aA26iWR0Sv6dhk8e+lOKaw=="
2018-12-12T12:52:15.660Z INF/provider 1 [*] Sending output to "tcp://devside.stf.ovwane.com:7270"
2018-12-12T12:52:15.662Z INF/provider 1 [*] Receiving input from "tcp://devside.stf.ovwane.com:7250"
2018-12-12T12:52:15.668Z INF/provider 1 [*] Tracking devices
2018-12-12T12:52:16.546Z INF/provider 1 [*] Found device "27423f88" (offline)
2018-12-12T12:52:16.566Z INF/provider 1 [*] Registered device "27423f88"
2018-12-12T12:52:16.567Z INF/provider 1 [*] Lost device "27423f88" (offline)
2018-12-12T12:52:17.573Z INF/provider 1 [*] Found device "27423f88" (offline)
2018-12-12T12:52:17.591Z INF/provider 1 [*] Registered device "27423f88"
2018-12-12T12:52:17.592Z INF/provider 1 [*] Lost device "27423f88" (offline)
```



# Ubuntu Docker 安装 OpenStf

### IP address

- proxy role: `10.8.8.128`
- database role: `10.8.8.128`
- app role: `10.8.8.128`
- provider1: `10.8.8.131`

/etc/hosts

```
10.8.8.128 stf.ovwane.com
10.8.8.128 devside.stf.ovwane.com
10.8.8.131 provider1.stf.ovwane.com
```



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
/data/nginx/conf/nginx.conf

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



dc.sh

```
cat > dc.sh<<EOF
#!/usr/bin/env bash

PWD=/root/stf_app

function stop(){
    cd $PWD
    docker-compose down
}

function start(){
    cd $PWD
    docker-compose up -d
}

\$1
EOF
```

```shell
chmod +x dc.sh
```

vim /etc/systemd/system/stf-app.service

```
[Unit]
Description=stf app
Wants=network.target
After=network.target

[Service]
Type=oneshot
ExecStartPre=/root/stf_app/dc.sh stop
ExecStart=/root/stf_app/dc.sh start
ExecStop=/root/stf_app/dc.sh start
RemainAfterExit=yes
StandardOutput=journal
StandardError=inherit

[Install]
WantedBy=multi-user.target
```

启动

```shell
systemctl enable stf-app.service
systemctl start stf-app.service
systemctl status stf-app.service
systemctl stop stf-app.service
```



# Provider role配置

## 拉取镜像

```shell
for name in sorccu/adb:latest openstf/stf:v3.4.0; do
	docker pull $name
done
```

> 每台provider都要启动adbd和stf-provider。

## ~~启动adbd~~

> 此方式添加新手机不能自动识别。

```shell
docker run -d --name adbd --privileged --net host -v /dev/bus/usb:/dev/bus/usb sorccu/adb:latest
```

## 使用ubuntu端adb

```shell
apt -y install adb
```



vim /etc/systemd/system/adbd.service  

```
[Unit]
Description=Android Debug Bridge daemon
After=network.target

[Service]
#TimeoutStartSec=1min
Restart=always
RestartSec=2s
Type=forking
User=root
ExecStartPre=/usr/bin/adb kill-server
ExecStart=/usr/bin/adb start-server
ExecStop=/usr/bin/adb kill-server

[Install]
WantedBy=multi-user.target
```

启动

```shell
systemctl enable adbd.service
systemctl start adbd.service
systemctl status adbd.service
```

dc.sh

```shell
cat > dc.sh<<EOF
#!/usr/bin/env bash

PWD=/root/stf_provider

function stop(){
    cd $PWD
    docker-compose down
}

function start(){
    cd $PWD
    docker-compose up -d
}

\$1
EOF
```

```shell
chmod +x dc.sh
```



vim /etc/systemd/system/stf-provider.service

```
[Unit]
Description=stf provider
After=adbd.service

[Service]
Type=oneshot
ExecStartPre=/root/stf_provider/dc.sh stop
ExecStart=/root/stf_provider/dc.sh start
ExecStop=/root/stf_provider/dc.sh stop
RemainAfterExit=yes
StandardOutput=journal
StandardError=inherit

[Install]
WantedBy=multi-user.target
```

启动

```shell
systemctl enable stf-provider.service
systemctl start stf-provider.service
systemctl status stf-provider.service
systemctl stop stf-provider.service
```

## 启动stf-provider1

**添加系统变量**

```shell
STF_VERSION=3.4.0 \
STF_HOST=stf.域名 \
SFT_URL=https://$STF_HOST \
STF_IMAGE=openstf/stf:v$STF_VERSION \
STF_PROVIDER=provider1
```

**启动stf-provider**

```shell
docker run -d --name stf-($STF_PROVIDER)-$STF_VERSION --net host $STF_IMAGE stf provider --name "${STF_PROVIDER}" --connect-sub tcp://devside.$STF_HOST:7250 --connect-push tcp://devside.$STF_HOST:7270 --storage-url $SFT_URL --public-ip $STF_PROVIDER.$STF_HOST --min-port=15000 --max-port=25000 --heartbeat-interval 20000 --screen-ws-url-pattern "ws://${STF_HOST}/d/${STF_PROVIDER}/<%= serial %>/<%= publicPort %>/"
```

## 启动stf-provider2

**添加系统变量**

```shell
STF_VERSION=3.4.0 \
STF_HOST=stf.域名 \
SFT_URL=https://$STF_HOST \
STF_IMAGE=openstf/stf:v$STF_VERSION \
STF_PROVIDER=provider2
```

**启动stf-provider**

```shell
docker run -d --name stf-($STF_PROVIDER)-$STF_VERSION --net host $STF_IMAGE stf provider --name "${STF_PROVIDER}" --connect-sub tcp://devside.$STF_HOST:7250 --connect-push tcp://devside.$STF_HOST:7270 --storage-url $SFT_URL --public-ip $STF_PROVIDER.$STF_HOST --min-port=15000 --max-port=25000 --heartbeat-interval 20000 --screen-ws-url-pattern "ws://${STF_HOST}/d/${STF_PROVIDER}/<%= serial %>/<%= publicPort %>/"
```



## 参考

[ STF 折腾之路 最后换成 Docker 来安装](https://testerhome.com/topics/10406)

[STF 正式环境 docker 化集群部署](https://testerhome.com/topics/12755)

[STF docker 集群部署，树莓派做子节点,附带完整配置](https://testerhome.com/topics/15027)

[Mac 上用 docker 安装 openstf--一步一坑从入门到放弃](https://testerhome.com/topics/12489)

[OpenStf Deployment](https://github.com/openstf/stf/blob/master/doc/DEPLOYMENT.md)

[STF docker 集群部署，树莓派做子节点,附带完整配置](https://testerhome.com/topics/15027)

[STF 开发环境搭建与制作 docker 镜像过程](https://testerhome.com/topics/5206)

