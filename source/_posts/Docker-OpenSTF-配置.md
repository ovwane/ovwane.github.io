---
title: Docker OpenSTF 配置
date: 2018-10-26 14:06:01
tags:
---

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

[解决Error response from daemon: Get https://registry-1.docker.io/v2](https://blog.csdn.net/quanqxj/article/details/79479943)

#### 启动镜像

- 启动一个数据库

  ```shell
  docker run -d --name rethinkdb-2.3.6 --net host -p 8821:8080 -p 28015:28015 -p 29015:29015 -v /data/docker/rethinkdb:/data rethinkdb:2.3.6
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
  docker run -d --name stf-3.4.0 --net host openstf/stf:v3.4.0 stf local --public-ip 10.8.8.108
  ```
  ```shell
  docker stop stf-3.4.0
  docker rm stf-3.4.0
  docker start stf-3.4.0
  ```

  

## 参考

[ STF 折腾之路 最后换成 Docker 来安装](https://testerhome.com/topics/10406)

[STF 正式环境 docker 化集群部署](https://testerhome.com/topics/12755)

[STF docker 集群部署，树莓派做子节点,附带完整配置](https://testerhome.com/topics/15027)

[Mac 上用 docker 安装 openstf--一步一坑从入门到放弃](https://testerhome.com/topics/12489)