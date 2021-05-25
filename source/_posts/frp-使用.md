---
title: frp 使用
date: 2018-08-14 10:35:12
tags:
---

# [frp](https://github.com/fatedier/frp)

## 服务端

下载 frp

```shell
wget https://github.com/fatedier/frp/releases/download/v0.21.0/frp_0.21.0_linux_amd64.tar.gz
```



使用`tar`指令解压tar.gz文件

```shell
tar -zxvf frp_*_linux_amd64.tar.gz -C /usr/local/
```



使用`cd`指令进入解压出来的文件夹

```shell
cd /usr/local/frp_*_linux_amd64/
```



外网主机作为服务端，可以删掉不必要的客户端文件，使用`rm`指令删除文件。

```shell
rm -f frpc frpc.ini
```



接下来要修改服务器配置文件，即`frps.ini`文件。使用`vi`指令对目标文件进行编辑。

打开`frps.ini`后可以看到默认已经有很多详细的配置和示范样例，该文章仅以达到内网穿透为目的，所以这里选择**删掉或注释掉里面的所有内容**，然后根据群晖的情况，按照官方的中文文档添加以下配置。（这里的操作都使用`vi`命令，关于`vi`命令的使用方式这里不作详细介绍，可以自行搜索相关使用方法。）

/etc/systemd/system/frps.service

```
[Unit]
Description=Frp Server Service
After=network.target

[Service]
Type=simple
User=nobody
Restart=on-failure
RestartSec=5s
ExecStart=/usr/bin/frps -c /etc/frp/frps.ini

[Install]
WantedBy=multi-user.target
```



/etc/frp/frps.ini

```
[common]
# frp监听的端口，用作服务端和客户端通信
bind_port = 7000
# KCP需要绑定一个UDP端口
kcp_bind_port = 7000
# 服务端通过此端口接监听和接收公网用户的http请求
vhost_http_port = 7080

# 开启dashboard,frp提供了一个控制台，可以通过这个端口访问到控制台。可查看frp当前有多少代理连接以及对应的状态
dashboard_port = 7500
# dashboard 用户名密码，默认都为 admin
dashboard_user = admin
dashboard_pwd = admir234ns

# 日志存放路径
log_file = /var/log/frps.log
#log_level = warn
log_max_days = 7

# 开启toke认证
authentication_method = token
authenticate_heartbeats = true
authenticate_new_work_conns = true
token = 1234hh56thu

# 开启prometheus监控
enable_prometheus = true

# 子域名
subdomain_host = xxx.com
```



`[common]`部分是必须有的配置，其中`bind_port`是自己设定的frp服务端端口，`vhost_http_port`是自己设定的http访问端口。



systemd 方式后台运行。

```
mv frps /usr/bin/
mv frps.service /etc/systemd/system/
touch /var/log/frps.log && chown nobody:nobody /var/log/frps.log 
```

```
systemctl enable frps && systemctl restart frps && systemctl status frps -l
```



## 客户端

下载 frp

```shell
wget https://github.com/fatedier/frp/releases/download/v0.21.0/frp_0.21.0_linux_amd64.tar.gz

tar -zxvf frp_*_linux_amd64.tar.gz -C /root/
```



配置

/etc/systemd/system/frpc.service

```
[Unit]
Description=Frp Client Service
After=network.target

[Service]
Type=simple
User=nobody
Restart=on-failure
RestartSec=5s
ExecStart=/usr/bin/frpc -c /etc/frp/frpc.ini
ExecReload=/usr/bin/frpc reload -c /etc/frp/frpc.ini

[Install]
WantedBy=multi-user.target
```



vi /etc/frp/frpc.ini

```ini
[common]
# common 字段内容只能重启frpc，重启frpc所有链接都会断开
# 部署frp服务端的公网服务器的ip
server_addr = IP地址
# 和服务端的bind_port保持一致
server_port = 7000
#protocol = KCP

# 热更新配置
admin_addr =127.0.0.1
admin_port =7400

# 开启token认证
authentication_method = token
authenticate_heartbeats = true
authenticate_new_work_conns = true
token = 1234hh56thuy78

# 日志存放路径
#log_file = /var/log/frpc.log
#log_level = warn
#log_max_days = 7
#
[ssh]
# tcp | udp | http | https | stcp | xtcp, default is tcp
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 22222

[http-a]
type = http
# local_port代表你想要暴露给外网的本地web服务端口
local_port = 8000
# subdomain 在全局范围内要确保唯一，每个代理服务的subdomain不能重名，否则会影响正常使用。
# 客户端的subdomain需和服务端的subdomain_host配合使用
subdomain = p

[http-b]
type = tcp
local_ip = 127.0.0.1
local_port = 8080
remote_port = 38080

[range: test_tcp]
type = tcp
local_ip = 127.0.0.1
local_port = 6000-6006,6007
remote_port = 6000-6006,6007
```

> 上面的配置和服务端是对应的。

`[common]`中的`server_addr`填frp服务端的ip（也就是外网主机的IP），`server_port`填frp服务端的`bind_prot`。

`[ssh]`中的`local_port`填群晖的ssh端口。

`[nas]`中的`type`对应服务端配置。`local_port`填群晖的DSM端口。`custom_domains`为要映射的域名，记得域名的A记录要解析到外网主机的IP。

`[web]`同上，`local_port`填群晖的web端口。这里创建了两个http反向代理是为了分别映射群晖两个重要的端口，`5000`和`80`，前者用于登录群晖管理，后者用于群晖的`Web Station`和`DS Photo`。



运行frp客户端

```
mv frpc /usr/bin/
mv frpc.service /etc/systemd/system/
```

```shell
systemctl enable frpc && systemctl restart frpc && systemctl status frpc -l
```

热更新配置

```
systemctl reload frpc && systemctl status frpc -l
```



现在可以用SSH通过`外网主机IP:6000`和群晖建立SSH连接。通过浏览器访问`no1.sunnyrx.com:8080`打开群晖nas的管理页面，访问`no2.sunnyrx.com:8080`打开群晖`Web Station`的网站，`DS Photo app`可以连接`no2.sunnyrx.com:8080`进入`DS Photo`管理。



## 参考

 [frp 中文文档](https://github.com/fatedier/frp/blob/master/README_zh.md)

[使用frp实现内网穿透 群晖NAS+frp发挥更大作用](http://www.sunnyrx.com/2016/10/21/simple-to-use-frp/)

 [本地 STF 内网穿透公网访问 · TesterHome](https://testerhome.com/topics/22617) 

 [阿里云服务器实现 frp 内网穿透 | WooOh's blog](https://cao0507.github.io/2018/09/18/%E9%98%BF%E9%87%8C%E4%BA%91%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%AE%9E%E7%8E%B0frp%E5%86%85%E7%BD%91%E7%A9%BF%E9%80%8F/) 

 [frp配置实践教程 - 简书](https://www.jianshu.com/p/09603d9e0b6c) 

 [frp - 《frp 中文文档》 - 书栈网 · BookStack](https://www.bookstack.cn/read/frp/README_zh.md) 

