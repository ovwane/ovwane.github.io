---
title: Docker OpenSTF 配置
date: 2018-10-26 14:06:01
tags:
---

拉取镜像

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
docker pull nginx:1.15.5-alpine
```

#### 启动镜像

- 先启动一个数据库

  ```shell
  docker run -d --name rethinkdb-2.3.6 -p 8821:8080 -p 28015:28015 -p 29015:29015 -v ~/docker/rethinkdb:/data rethinkdb:2.3.6
  ```

- 再启动adb service

  ```shell
  docker run -d --name adbd --privileged -v /dev/bus/usb:/dev/bus/usb --net host sorccu/adb:latest
  ```

- 再启动stf 启动的时配置的IP地址为你虚拟机桥接的网址 enp0s3 

  ```shell
  docker run -d --name stf-3.4.0 openstf/stf:v3.4.0 stf local --public-ip 0.0.0.0
  ```

## 参考

[ STF 折腾之路 最后换成 Docker 来安装](https://testerhome.com/topics/10406)

[STF 正式环境 docker 化集群部署](https://testerhome.com/topics/12755)

[STF docker 集群部署，树莓派做子节点,附带完整配置](https://testerhome.com/topics/15027)

[Mac 上用 docker 安装 openstf--一步一坑从入门到放弃](https://testerhome.com/topics/12489)