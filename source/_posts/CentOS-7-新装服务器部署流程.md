---
title: CentOS 7 新装服务器部署流程
date: 2017-11-12 19:31:40
tags:
- CentOS
---
[CentOS 7 新装服务器部署流程](http://wzlinux.blog.51cto.com/8021085/1945374)

所有的服务器基本都是最小化安装，版本为CentOS 7.x  64位系列。

### 设置时区
```
timedatectl list-timezones           #列出所有时区
timedatectl set-local-rtc 1          #将硬件时钟调整为与本地时钟一致,0为设置为UTC时间
timedatectl set-timezone Asia/Shanghai #设置系统时区为上海
```

### 配置IP
配置内网IP    （如果是外网IP，linux要修改远程端口）

### 配置自己的yum源
```
yum install wget
rm -f /etc/yum.repos.d/*
wget -P /etc/yum.repos.d/ http://mirrors.163.com/.help/CentOS7-Base-163.repo
wget -P /etc/yum.repos.d/ https://mirrors.aliyun.com/repo/epel-7.repo

如何需要最新版本的rpm包，请安装下面的仓库
rpm -Uvh http://repo.webtatic.com/yum/el7/epel-release.rpm
rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
```

### 关闭SELinux      
```
sed -i 's#SELINUX=enforcing#SELINUX=disabled#g' /etc/selinux/config
setenforce 0
```

### 添加zabbix监控
### 配置防火墙
```
systemctl stop firewalld.service
systemctl disable firewalld.service
```

### 安装软件包  
```
yum install -y vim openssh-clients ntpdate man lrzsz net-tools
```

### 时间同步任务
```
chrony
```
```
echo "10 6 * * * root (/usr/sbin/ntpdate time1.aliyun.com && /sbin/hwclock -w) &> /dev/null" >> /etc/crontab
```

### 配置主机名
```
vim /etc/hostname
```

### 修改文件句柄数  
```
#临时修改，立刻生效
ulimit -n 655350         
 
#永久修改
echo "* soft nofile 655360" >> /etc/security/limits.conf
echo "* hard nofile 655360" >> /etc/security/limits.conf
```

### 禁用ipv6(可选）
```
cat >> /etc/modprobe.d/ipv6.conf <<EOF
alias net-pf-10 off
alias ipv6 off
EOF
```

### 去除ssh远程DNS认证
```
sed -i 's/#UseDNS yes/UseDNS no/g' /etc/ssh/sshd_config
sed -i 's/GSSAPIAuthentication yes/GSSAPIAuthentication no/g' /etc/ssh/sshd_config
```
```
systemctl restart sshd.service
```

### 配置LDAP客户端（可选）
```
yum install openldap-clients nss-pam-ldapd -y

authconfig --enablemkhomedir \
--disableldaptls \
--enablemd5 \
--enableldap \
--enableldapauth \
--ldapserver=ldap://211.x.x.27:8389 \
--ldapbasedn="dc=wzlinux,dc=com" \
--enableshadow \
--update
```

### 配置国内pip源
创建~/.pip/pip.conf，填写如下内容 

```
[global] 
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
[install]
trusted-host=mirrors.aliyun.com
```