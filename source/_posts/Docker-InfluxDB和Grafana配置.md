---
title: Docker InfluxDB和Grafana配置
date: 2018-10-24 09:56:38
tags:
---



拉取镜像

```shell
docker pull samuelebistoletti/docker-statsd-influxdb-grafana:2.1.0
```

启动镜像

```shell
docker run --ulimit nofile=66000:66000 \
  -d \
  --name docker-statsd-influxdb-grafana-2.1.0 \
  -p 3003:3003 \
  -p 3004:8888 \
  -p 8086:8086 \
  -p 22022:22 \
  -p 8125:8125/udp \
  samuelebistoletti/docker-statsd-influxdb-grafana:2.1.0
```

关闭启动

```shell
docker stop docker-statsd-influxdb-grafana

docker start docker-statsd-influxdb-grafana
```

端口映射关系

## Mapped Ports

```
Host		Container		Service

3003		3003			grafana
3004		8888			influxdb-admin (chronograf)
8086		8086			influxdb
8125		8125			statsd
22022		22              sshd
```

## SSH

```
ssh root@localhost -p 22022
```

Password: root

## Grafana

Open [http://localhost:3003](http://localhost:3003/)

```
Username: root
Password: root 
```

### Add data source on Grafana

1. Using the wizard click on `Add data source`
2. Choose a `name` for the source and flag it as `Default`
3. Choose `InfluxDB` as `type`
4. Choose `direct` as `access`
5. Fill remaining fields as follows and click on `Add` without altering other fields

```
Url: http://localhost:8086
Database:	telegraf
User: telegraf
Password:	telegraf
```

Basic auth and credentials must be left unflagged. Proxy is not required.

Now you are ready to add your first dashboard and launch some query on database.

## InfluxDB

### Web Interface

Open [http://localhost:3004](http://localhost:3004/)

```
Username: root
Password: root
Port: 8086
```

### InfluxDB Shell (CLI)

1. Establish a ssh connection with the container
2. Launch `influx` to open InfluxDB Shell (CLI)

## 数据收集

**安装TeleGraf**

```shell
brew install telegraf
```

**设置**

/etc/telegraf/telegraf.conf
修改influxdb地址，用户名及密码，设置hostname

```
telegraf -config /usr/local/etc/telegraf.conf
```

**重启服务**

```shell
 brew services restart telegraf
```

**导入Grafana Dashboard**

下载最新版本的dashboard配置：
<https://grafana.com/dashboards/1443/revisions>

在grafana的新建dashboard并导入配置，完成。

**注意**

docker内部已经启动了telegraf，如果不需要的话可以停掉，在多台服务器上安装并配置Telegraf写入同一Influxdb就可以实现对集群进行系统监控。

## 参考

[ Docker Image with Telegraf (StatsD), InfluxDB and Grafana](https://github.com/samuelebistoletti/docker-statsd-influxdb-grafana)

[Docker+Grafana+Influxdb+Telegraf安装部署](http://xuqi365.com/2017/08/14/docker-grafana-influxdb-telegraf%E5%AE%89%E8%A3%85%E9%83%A8%E7%BD%B2/)

[Telegraf+InfluxDB+Grafana搭建服务器监控平台](https://blog.csdn.net/w958660278/article/details/80484486)

[[grafana + influxdb + telegraf , 构建性能监控平台](https://www.cnblogs.com/Scissors/p/5977670.html)](http://www.cnblogs.com/Scissors/p/5977670.html)

[Grafana部署-展示Zabbix数据](https://www.jianshu.com/p/d902f8bc59e3)

