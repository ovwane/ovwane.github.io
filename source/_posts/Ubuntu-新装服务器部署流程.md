---
title: Ubuntu 新装服务器部署流程
date: 2017-12-18 15:02:01
categories:
- 技术
- Linux
tags:
- Ubuntu
---
# Ubuntu 新装服务器部署流程
>Ubuntu 16.04.3

[Ubuntu 新装服务器部署流程](http://wzlinux.blog.51cto.com/8021085/1964616)
[Ubuntu 新装服务器部署流程](http://blog.51cto.com/wzlinux/2043586)

### 配置IP
修改文件/etc/network/interfaces

```
auto ens33
iface ens33 inet static
address 192.168.8.100
netmask 255.255.255.0
gateway 192.168.8.1
dns-nameserver 8.8.8.8
```

重启网络

```
systemctl restart netwoking.service
/etc/init.d/networking restart
```

### 设定时区
```
rm -f /etc/localtime
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
date -R
```

### 配置apt-get源
ubuntu 14.04

```
rm -f /etc/apt/sources.list
wget -P /etc/apt/ http://mirrors.163.com/.help/sources.list.trusty
mv /etc/apt/sources.list.trusty /etc/apt/sources.list
```
 
ubuntu 16.04
[清华](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

```
cd /etc/apt
cp source.list source.list.bak

vi source.list

# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ xenial-proposed main restricted universe multiverse
```

### 修改主机名
```
vim /etc/hostname
```

### 安装软件包
```bash
apt-get update
apt-get install -y openssh-server vim lrzsz git tree bash-completion sysstat
``` 

### 开启root登录
```
vi /etc/ssh/sshd_config
PermitRootLogin yes
```
```
service ssh restart
systemctl restart sshd
```

### 踢出终端tty用户
```
pkill -kill -t tty
```