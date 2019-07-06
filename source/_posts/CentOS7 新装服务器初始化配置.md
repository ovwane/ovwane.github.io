---
title: CentOS7 新装服务器初始化配置
date: 2017-11-17 18:52:45
categories:
- 技术
- Linux
tags:
- CentOS
---
# CentOS7 新装服务器初始化配置
>CentOS 7.4.1708

[CentOS 7 新装服务器部署流程](http://wzlinux.blog.51cto.com/8021085/1945374)

[Questions about CentOS-7](https://wiki.centos.org/FAQ/CentOS7)

所有的服务器基本都是最小化安装，版本为CentOS 7.x  64位系列。



## 配置IP和网关
```
vi /etc/sysconfig/network-scripts/ifcfg-eth0

ONBOOT=yes
IPADDR=10.0.0.7
PREFIX=24
GATEWAY=10.0.0.1
DNS1=114.114.114.114
```
```shell
# 重启网络服务
systemctl restart network.service
```

> [2.2. Editing Network Configuration Files](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Networking_Guide/sec-Editing_Network_Configuration_Files.html)



## 设置时区

```
timedatectl list-timezones           #列出所有时区
timedatectl set-local-rtc 1          #将硬件时钟调整为与本地时钟一致,0为设置为UTC时间
timedatectl set-timezone Asia/Shanghai #设置系统时区为上海
```

## 设置主机名
```
#设置一个主机的变量因为主机名从node1-node9
a=1
#$a为变量
hostnamectl set-hostname node$a.ovwane.com
```

## 关闭SELinux
```
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

## 关闭防火墙
```
systemctl stop firewalld&&systemctl disable firewalld
```

## 关闭IPv6
```
sed -i 's/^GRUB_CMDLINE_LINUX=\"/GRUB_CMDLINE_LINUX=\"ipv6.disable=1 /g' /etc/default/grub&&
grub2-mkconfig -o /boot/grub2/grub.cfg
```

## 更改YUM源
```
rm /etc/yum.repos.d/* -rf;curl -ssL http://mirrors.163.com/.help/CentOS7-Base-163.repo>/etc/yum.repos.d/CentOS-Base.repo;echo $?
```

## 时间同步
```
yum install -y chrony;systemctl enable chronyd.service&&systemctl start chronyd.service
```

## 常用软件
```
yum install -y lrzsz wget vim git tree bash-completion sysstat lsof net-tools unzip
```

## VIM安装配置

## 修改文件句柄数  
```
#临时修改，立刻生效
ulimit -n 655360         
 
#永久修改
echo "* soft nofile 655360" >> /etc/security/limits.conf&&echo "* hard nofile 655360" >> /etc/security/limits.conf
```

## SSH设置
```
sed -i 's/#UseDNS no/UseDNS no/g' /etc/ssh/sshd_config
```

## 重启
```
shutdown -r now
```