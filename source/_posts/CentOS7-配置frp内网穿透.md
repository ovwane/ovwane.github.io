---
title: CentOS7 配置frp内网穿透
date: 2018-08-14 10:35:12
tags:
---

[下载frp](https://github.com/fatedier/frp/releases)

### 服务端 外网主机

SSH连接上外网主机后，使用`wget`指令下载frp。

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

```
vi frps.ini
```

打开`frps.ini`后可以看到默认已经有很多详细的配置和示范样例，该文章仅以达到内网穿透为目的，所以这里选择**删掉或注释掉里面的所有内容**，然后根据群晖的情况，按照官方的中文文档添加以下配置。（这里的操作都使用`vi`命令，关于`vi`命令的使用方式这里不作详细介绍，可以自行搜索相关使用方法。）

```
[common]
bind_port = 7000
vhost_http_port = 8080
```

`[common]`部分是必须有的配置，其中`bind_port`是自己设定的frp服务端端口，`vhost_http_port`是自己设定的http访问端口。

保存上面的配置后，使用以下指令启动frp服务端。

```shell
#新建会话
screen -dmS frp

#然后进入这个会话。
screen -r frp

#执行命令
./frps -c ./frps.ini &
```

服务端的工作就到此结束了。

### 客户端

客户端前面的操作和服务端是一模一样的，这里不一一解释。

```shell
wget https://github.com/fatedier/frp/releases/download/v0.21.0/frp_0.21.0_linux_amd64.tar.gz

tar -zxvf frp_*_linux_amd64.tar.gz -C /usr/local/

cd /usr/local/frp_*_linux_amd64

rm -f frps frps.ini
```

客户端的配置如下

vi frpc.ini

```
[common]
server_addr = x.x.x.x
server_port = 7000

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22
remote_port = 6000

[nas]
type = http
local_port = 5000
custom_domains = no1.sunnyrx.com

[web]
type = http
local_port = 80
custom_domains = no2.sunnyrx.com
```

上面的配置和服务端是对应的。

`[common]`中的`server_addr`填frp服务端的ip（也就是外网主机的IP），`server_port`填frp服务端的`bind_prot`。

`[ssh]`中的`local_port`填群晖的ssh端口。

`[nas]`中的`type`对应服务端配置。`local_port`填群晖的DSM端口。`custom_domains`为要映射的域名，记得域名的A记录要解析到外网主机的IP。

`[web]`同上，`local_port`填群晖的web端口。这里创建了两个http反向代理是为了分别映射群晖两个重要的端口，`5000`和`80`，前者用于登录群晖管理，后者用于群晖的`Web Station`和`DS Photo`。

保存配置，输入以下指令运行frp客户端。（同样如果需要在后台运行，请往下翻阅关于后台运行的部分。）

```shell
./frpc -c ./frpc.ini
```

此时在服务端会看到”start proxy sucess”字样，即连接成功。

现在可以用SSH通过`外网主机IP:6000`和群晖建立SSH连接。通过浏览器访问`no1.sunnyrx.com:8080`打开群晖nas的管理页面，访问`no2.sunnyrx.com:8080`打开群晖`Web Station`的网站，`DS Photo app`可以连接`no2.sunnyrx.com:8080`进入`DS Photo`管理。

###### 使用nohup指令

nohup指令的使用方法相对简单，只需要在`nohup`后面加上frp的运行指令即可。下面示范的指令是运行frp客户端。（同样，如果之前断开了SSH连接，记得用`cd`指令进入frp的目录先。）

```shell
nohup ./frpc -c ./frpc.ini &
```

这样就成功让frp在后台运行了。

## 参考

[使用frp实现内网穿透 群晖NAS+frp发挥更大作用](http://www.sunnyrx.com/2016/10/21/simple-to-use-frp/)