---
title: Ubuntu 新装服务器部署流程
date: 2017-11-12 19:32:01
categories:
- 技术
- Linux
tags:
- Ubuntu
---
[Ubuntu 新装服务器部署流程](http://wzlinux.blog.51cto.com/8021085/1964616)

### 配置IP
修改文件/etc/network/interfaces

```
# The primary network interface
auto eth0
iface eth0 inet static
address  10.0.0.100
netmask  255.0.0.0
gateway  10.0.0.1
dns-nameservers 114.114.114.114
```

### 设定时区
```
rm -f /etc/localtime
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

### 配置apt-get源
#### 版本 14.04
```
rm -f /etc/apt/sources.list
wget -P /etc/apt/ http://mirrors.163.com/.help/sources.list.trusty
mv /etc/apt/sources.list.trusty /etc/apt/sources.list
 ```
 
#### 版本 16.04
```
sed -i 's/us.archive/cn.archive/g' /etc/apt/sources.list
sed -i 's/security.ubuntu/cn.archive.ubuntu/g' /etc/apt/sources.list
```

### 修改主机名
```
vim /etc/hostname
```

### 安装软件包
```
apt-get install lrzsz ntpdate
```

### 开启root登录
```
编辑/etc/ssh/sshd_config，（首先配置好密码）
把
PermitRootLogin prohibit-password
改为
PermitRootLogin yes
```
```
service ssh restart
```

### 踢出终端tty用户
```
pkill -kill -t tty
```

### 删除普通用户
### 安装监控系统
```
apt-get install zabbix-agent
```

### 配置pip国内源
创建~/.pip/pip.conf，填写如下内容

```
[global]
trusted-host =  pypi.douban.com
index-url = http://pypi.douban.com/simple
```