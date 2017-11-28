---
title: CentOS 6 新装服务器部署流程
date: 2017-11-12 19:31:45
categories:
- 技术
- Linux
tags:
- CentOS
---
[CentOS（5.8/6.4）linux生产环境若干优化实战](http://oldboy.blog.51cto.com/2561410/1336488)[CentOS 6 新装服务器部署流程](http://wzlinux.blog.51cto.com/8021085/1704772)[实践生产服务器环境最小化安装后 Centos 6.5 优化](http://maocong.blog.51cto.com/4706737/1682372)

特别说明：本文来自老男孩linux培训VIP学生学习笔记。特和所有博友分享。更多优化，请关注老男孩培训后续课程内容以及分享。
为了满足广大博友工作需求，老男孩linux培训课程从2013年起已经同时适合Centos5.X和Centos6.X!
CentOS系统安装之后并不能立即投入生产环境使用，往往需要先经过我们运维人员的优化才行。在此讲解几点关于Linux系统安装后的基础优化操作。注意：本次优化都是基于CentOS（5.8/6.4）。
下面我就为大家简单讲解几点关于Linux系统安装后的基础优化操作。
注意：本次优化都是基于CentOS（5.8/6.4）。关于5.8和6.4两者优化时的小区别，我会在文中提及的。
优化条目：

修改ip地址、网关、主机名、DNS等
关闭selinux，清空iptables
添加普通用户并进行sudo授权管理
更新yum源及必要软件安装
定时自动更新服务器时间
精简开机自启动服务
定时自动清理/var/spool/clientmqueue/目录垃圾文件，放置inode节点被占满
变更默认的ssh服务端口，禁止root用户远程连接
锁定关键文件系统
调整文件描述符大小
调整字符集，使其支持中文
去除系统及内核版本登录前的屏幕显示
内核参数优化
1、修改ip地址、网关、主机名、DNS等

```
[root@localhost ~]# vi /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0         #网卡名字
BOOTPROTO=static    #静态IP地址获取状态 如：DHCP表示自动获取IP地址
IPADDR=192.168.1.113            #IP地址
NETMASK=255.255.255.0           #子网掩码
ONBOOT=yes#引导时是否激活
GATEWAY=192.168.1.1
[root@localhost ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0
BOOTPROTO=static
IPADDR=192.168.1.113
NETMASK=255.255.255.0
ONBOOT=yes
GATEWAY=192.168.1.1
```

[root@localhost ~]# vi /etc/sysconfig/network
HOSTNAME=c64     #修改主机名，重启生效
GATEWAY=192.168.1.1    #修改默认网关,如果上面eth0里面不配置网关的话，默认就使用这里的网关了。
[root@localhost ~]# cat /etc/sysconfig/network
HOSTNAME=c64
GATEWAY=192.168.1.1
我们也可以用  hostnamec64  来临时修改主机名，重新登录生效
修改DNS
[root@localhost ~]# vi /etc/resolv.conf   #修改DNS信息
nameserver 114.114.114.114
nameserver 8.8.8.8
[root@localhost ~]# cat /etc/resolv.conf  #查看修改后的DNS信息
nameserver 114.114.114.114
nameserver 8.8.8.8
[root@localhost ~]# service network restart   #重启网卡，生效
重启网卡，也可以用下面的命令
[root@localhost ~]# /etc/init.d/network restart


```
rm -f /etc/localtime
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```





3、添加普通用户并进行sudo授权管理

```
[root@c64 ~]# useradd sunsky
[root@c64 ~]# echo "123456"|passwd --stdin sunsky&&history –c
[root@c64 ~]# visudo
在root    ALL=(ALL)    ALL此行下，添加如下内容
sunsky    ALL=(ALL)    ALL
```


1
[root@c64 yum.repos.d]# yum install lrzsz ntpdate sysstat -y
lrzsz是一个上传下载的软件
sysstat是用来检测系统性能及效率的工具

```
5、定时自动更新服务器时间
```
[root@c64 ~]# echo '*/5 * * * * /usr/sbin/ntpdate time.windows.com >/dev/null 2 >&1' >>/var/spool/cron/root
[root@c64 ~]# echo '*/10 * * * * /usr/sbin/ntpdate time.nist.gov >/dev/null 2>&1' >>/var/spool/cron/root
提示：CentOS 6.4的时间同步命令路径不一样
6是/usr/sbin/ntpdate
5是/sbin/ntpdate
```

扩展：在机器数量少时，以上定时任务同步时间就可以了。如果机器数量大时，可以在网内另外部署一台时间同步服务器NTP Server。此处仅提及，不做部署。
时间同步服务器架构图：

 
6、精简开机自启动服务
刚装完操作系统可以只保留crond，network，syslog，sshd这四个服务。（Centos6.4为rsyslog）

```
[root@c64 ~]# for sun in `chkconfig --list|grep 3:on|awk '{print $1}'`;do chkconfig --level 3 $sun off;done
[root@c64 ~]# for sun in crond rsyslog sshd network;do chkconfig --level 3 $sun on;done
[root@c64 ~]# chkconfig --list|grep 3:on
crond           0:off   1:off   2:on    3:on    4:on    5:on    6:off
network         0:off   1:off   2:on    3:on    4:on    5:on    6:off
rsyslog         0:off   1:off   2:on    3:on    4:on    5:on    6:off
sshd            0:off   1:off   2:on    3:on    4:on    5:on    6:off
```

7、定时自动清理/var/spool/clientmqueue/目录垃圾文件，放置inode节点被占满
本优化点，在6.4上可以忽略不需要操作即可！

```
[root@c64 ~]# mkdir /server/scripts -p
[root@c64 ~]# vi /server/scripts/spool_clean.sh
#!/bin/sh
find/var/spool/clientmqueue/-typef -mtime +30|xargsrm-f
然后将其加入到crontab定时任务中
1
[root@c64 ~]# echo '*/30 * * * * /bin/sh /server/scripts/spool_clean.sh >/dev/null 2>&1'>>/var/spool/cron/root
```

8、变更默认的ssh服务端口，禁止root用户远程连接

```
[root@c64 ~]# cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
[root@c64 ~]# vim /etc/ssh/sshd_config
Port 52113#ssh连接默认的端口
PermitRootLogin no   #root用户黑客都知道，禁止它远程登录
PermitEmptyPasswords no #禁止空密码登录
UseDNS no            #不使用DNS
[root@c64 ~]# /etc/init.d/sshd reload    #从新加载配置
[root@c64 ~]# netstat -lnt     #查看端口信息
[root@c64 ~]# lsof -i tcp:52113
```

9、锁定关键文件系统

```
[root@c64 ~]# chattr +i /etc/passwd
[root@c64 ~]# chattr +i /etc/inittab
[root@c64 ~]# chattr +i /etc/group
[root@c64 ~]# chattr +i /etc/shadow
[root@c64 ~]# chattr +i /etc/gshadow
使用chattr命令后，为了安全我们需要将其改名
1
[root@c64 ~]# /bin/mv /usr/bin/chattr /usr/bin/任意名称
```

```
扩展：文件描述符
文件描述符在形式上是一个非负整数。实际上，它是一个索引值，指向内核为每一个进程所维护的该进程打开文件的记录表。当程序打开一个现有文件或者创建一个新文件时，内核向进程返回一个文件描述符。在程序设计中，一些涉及底层的程序编写往往会围绕着文件描述符展开。但是文件描述符这一概念往往只适用于Unix、Linux这样的操作系统。
习惯上，标准输入（standard input）的文件描述符是 0，标准输出（standard output）是 1，标准错误（standard error）是 2。尽管这种习惯并非Unix内核的特性，但是因为一些 shell 和很多应用程序都使用这种习惯，因此，如果内核不遵循这种习惯的话，很多应用程序将不能使用。
 
11、调整字符集，使其支持中文

```
sed-i 's#LANG="en_US.UTF-8"#LANG="zh_CN.GB18030"#'/etc/sysconfig/i18n
source/etc/sysconfig/i18n
扩展：什么是字符集？
简单的说就是一套文字符号及其编码。常用的字符集有：
GBK 定长双字节不是国际标准，支持系统不少
UTF-8 非定长 1-4字节广泛支持，MYSQL也使用UTF-8
```

12、去除系统及内核版本登录前的屏幕显示

```
[root@c64 ~]# >/etc/redhat-release
[root@c64 ~]# >/etc/issue
```

13、内核参数优化
说明：本优化适合apache，nginx，squid多种等web应用，特殊的业务也可能需要略作调整。

```
[root@c64 ~]# vi /etc/sysctl.conf
#by sun in 20131001
net.ipv4.tcp_fin_timeout = 2
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_keepalive_time =600
net.ipv4.ip_local_port_range = 4000    65000
net.ipv4.tcp_max_syn_backlog = 16384
net.ipv4.tcp_max_tw_buckets = 36000
net.ipv4.route.gc_timeout = 100
net.ipv4.tcp_syn_retries = 1
net.ipv4.tcp_synack_retries = 1
net.core.somaxconn = 16384
net.core.netdev_max_backlog = 16384
net.ipv4.tcp_max_orphans = 16384
#一下参数是对iptables防火墙的优化，防火墙不开会有提示，可以忽略不理。
net.ipv4.ip_conntrack_max = 25000000
net.ipv4.netfilter.ip_conntrack_max = 25000000
net.ipv4.netfilter.ip_conntrack_tcp_timeout_established = 180
net.ipv4.netfilter.ip_conntrack_tcp_timeout_time_wait = 120
net.ipv4.netfilter.ip_conntrack_tcp_timeout_close_wait = 60
net.ipv4.netfilter.ip_conntrack_tcp_timeout_fin_wait = 120
[root@localhost ~]# sysctl –p    #使配置文件生效
```
提示：由于CentOS6.X系统中的模块名不是ip_conntrack，而是nf_conntrack，所以在/etc/sysctl.conf优化时，需要把net.ipv4.netfilter.ip_conntrack_max 这种老的参数，改成net.netfilter.nf_conntrack_max这样才可以。
即对防火墙的优化，在5.8上是

net.ipv4.ip_conntrack_max = 25000000
net.ipv4.netfilter.ip_conntrack_max = 25000000
net.ipv4.netfilter.ip_conntrack_tcp_timeout_established = 180
net.ipv4.netfilter.ip_conntrack_tcp_timeout_time_wait = 120
net.ipv4.netfilter.ip_conntrack_tcp_timeout_close_wait = 60
net.ipv4.netfilter.ip_conntrack_tcp_timeout_fin_wait = 120
在6.4上是

net.nf_conntrack_max = 25000000
net.netfilter.nf_conntrack_max = 25000000
net.netfilter.nf_conntrack_tcp_timeout_established = 180
net.netfilter.nf_conntrack_tcp_timeout_time_wait = 120
net.netfilter.nf_conntrack_tcp_timeout_close_wait = 60
net.netfilter.nf_conntrack_tcp_timeout_fin_wait = 120
另外，在此优化过程中可能会有报错：
1、5.8版本上

error: "net.ipv4.ip_conntrack_max"is an unknown key
error: "net.ipv4.netfilter.ip_conntrack_max"is an unknown key
error: "net.ipv4.netfilter.ip_conntrack_tcp_timeout_established"is an unknown key
error: "net.ipv4.netfilter.ip_conntrack_tcp_timeout_time_wait"is an unknown key
error: "net.ipv4.netfilter.ip_conntrack_tcp_timeout_close_wait"is an unknown key
error: "net.ipv4.netfilter.ip_conntrack_tcp_timeout_fin_wait"is an unknown key
这个错误可能是你的防火墙没有开启或者自动处理可载入的模块ip_conntrack没有自动载入，解决办法有二，一是开启防火墙，二是自动处理开载入的模块ip_conntrack
modprobe ip_conntrack
echo "modprobe ip_conntrack">> /etc/rc.local
2、6.4版本上

error: "net.nf_conntrack_max"isan unknown key
error: "net.netfilter.nf_conntrack_max"isan unknown key
error: "net.netfilter.nf_conntrack_tcp_timeout_established"isan unknown key
error: "net.netfilter.nf_conntrack_tcp_timeout_time_wait"isan unknown key
error: "net.netfilter.nf_conntrack_tcp_timeout_close_wait"isan unknown key
error: "net.netfilter.nf_conntrack_tcp_timeout_fin_wait"isan unknown key
这个错误可能是你的防火墙没有开启或者自动处理可载入的模块ip_conntrack没有自动载入，解决办法有二，一是开启防火墙，二是自动处理开载入的模块ip_conntrack

modprobe nf_conntrack
echo "modprobe nf_conntrack">> /etc/rc.local
3、6.4版本上

error: "net.bridge.bridge-nf-call-ip6tables"isan unknown key
error: "net.bridge.bridge-nf-call-iptables"isan unknown key
error: "net.bridge.bridge-nf-call-arptables"isan unknown key

这个错误是由于自动处理可载入的模块bridge没有自动载入，解决办法是自动处理开载入的模块ip_conntrack
1
2
modprobe bridge
echo "modprobe bridge">> /etc/rc.local
到此，我们Linux系统安装后的基础优化已经操作的差不多了，总结下来一共有13个优化点需要我们来熟知。后面我会出一个一键优化的shell脚本出来和大家一起交流学习。






所有的服务器基本都是最小化安装，版本为CentOS 6.x  64位系列。

### 设置时区
```
rm -f /etc/localtime
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

### 系统时间更新和设定定时任务 
第一种：更新时间并且写入BOIS

```
ntpdate time.windows.com && hwclock -w && hwclock --systohc
```

第二种：更新时间并且写入定时任务

```
echo '*/30 * * * * ntpdate time.windows.com && hwclock -w && hwclock --systohc >/dev/null 2>&1' >>/var/spool/cron/root
```

第三种：每间隔5分钟和10分钟同步一次时间

```
echo '*/5 * * * * /usr/sbin/ntpdate time.windows.com >/dev/null 2 >&1' >>/var/spool/cron/root
echo '*/10 * * * * /usr/sbin/ntpdate time.nist.gov >/dev/null 2>&1' >>/var/spool/cron/root
```
提示：CentOS 6.x的时间同步命令路径不一样 6是/usr/sbin/ntpdate 5是/sbin/ntpdate

### 配置IP 
配置内网IP（如果是外网IP，linux要修改远程SSH端口）
修改ip地址、网关、主机名、DNS #eth0 网卡设置
```
```
#修改ip地址

mv /etc/sysconfig/network-scripts/ifcfg-eth0 /etc/sysconfig/network-scripts/ifcfg-eth0.bak
vi /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE=eth0 #网卡设备名称
ONBOOT=yes #是否启动引导的时候激活YES
NM_CONTROLLED=no #设备eth0是否可以由Network Manager图形管理工具托管
BOOTPROTO=dhcp #静态IP地址获取状态 如：DHCP表示自动获取IP地址
IPADDR=192.168.1.10 #IP
NETMASK=255.255.255.0 #网卡对应的网络掩码
```
```
#网关配置

vi /etc/sysconfig/network
#表示系统是否使用网络，一般设置为yes。如果设为no，则不能使用网络，而且很多系统服务程序将无法启动
NETWORKING=yes
#设置本机的主机名，这里设置的主机名要和/etc/hosts中设置的主机名对应
HOSTNAME=c65mini.localdomain
#设置本机连接的网关的IP地址。例如，网关为10.0.0.1或者192.168.1.1
GATEWAY=192.168.1.1
```
```
#修改主机DNS

vi /etc/resolv.conf
nameserver 8.8.8.8
nameserver 4.4.4.4
```
```
#修改HOSTS

vi /etc/hosts
127.0.0.1 c65mini.localdomain
#使用DNS域名服务器来解析名字
order bind hosts
#一台主机是否存在多个IP
multi on
#如果用逆向解析找出与指定的地址匹配的主机名，对返回的地址进行解析以确认它确实与您查询的地址相配。为了防止“骗取”IP地址
nospoof on
```
重启网卡生效设置两种方法

```
service network restart
或者
/etc/init.d/network restart
```
#查看selinux状态

第一种方法：/usr/bin/setstatus -v #如果显示：SELinux status: enabled 就是开启状态
第二种方法：cat /etc/selinux/config #如果显示：SELINUX=enforcing 则是开启状态permissive有提醒的状态 disabled是关闭
第三种方法：grep SELINUX=disabled /etc/selinux/config
第四种方法：getenforce

修改selinux状态 如果修改配置文件则永久生效，但是必须要重启系统
```

### 添加zabbix监控
### 配置防火墙
```
service iptables stop
iptables -L
service iptables save
```

### 安装软件包  
```
yum install -y vim openssh-clients ntpdate man
```

### 配置主机名
### 修改文件句柄数  
```
#临时修改，立刻生效
ulimit -n 655350         
 
#永久修改
echo "* soft nofile 655360" >> /etc/security/limits.conf
echo "* hard nofile 655360" >> /etc/security/limits.conf
```
```
#查看文件描述符大小
ulimit -n

第一种：#这里参考的是阿里云主机默认设置。
vi /etc/security/limits.conf
* soft nofile 65535
* hard nofile 65535
* soft nproc 65535
* hard nproc 65535
* soft nofile 65535
* hard nofile 65535

第二种：echo '* - nofile 65535' >> /etc/security/limits.conf

第三种：把ulimit -SHn 65535命令加入到/etc/rc.local，然后每次重启生效 追加命令到rc.local配置文件里面

cat >>/etc/rc.local<<EOF
#open files
ulimit -HSn 65535
#stack size
ulimit -s 65535
EOF

第四种：如果不修改limits配置文件，直接立即生效，但重启后又恢复之前的默认。 ulimit -SHn 65535
```


### 创建普通用户并进行sudo授权管理
```
创建普通用户 useradd bingoku 修改用户密码 passwd bingoku
另一种方式：一次性创建用户和设置密码 echo "123456"|passwd --stdin bingoku&&history –c
其中bingoku为你创建的用户名
```

sudo授权管理 打开sudo配置文件 visudo

```
#按:set nu 查看行，找到99行
root ALL=(ALL) ALL
#添加
bingoku ALL=(ALL) ALL
```

### 修改SSH端口号和屏蔽root账号远程登陆
```
#备份SSH配置
cp /etc/ssh/sshd_config sshd_config_bak
#修改SSH安全配置
vi /etc/ssh/sshd_config
#SSH链接默认端口
port 52113
#禁止root账号登陆
PermitRootLogin no
#禁止空密码
PermitEmptyPasswords no
#不使用DNS
UseDNS no
```
重新载入SSH配置 /etc/init.d/sshd reload 查看端口里面是否有刚才修改过的端口号52113

```
netstat -lnt
 
或者反查端口是那个进程
lsof -i tcp:52113
 ```

### 锁定关键文件系统（禁止非授权用户获得权限）
```
chattr +i /etc/passwd
chattr +i /etc/inittab
chattr +i /etc/group
chattr +i /etc/shadow
chattr +i /etc/gshadow
```

### 配置LDAP客户端(可选)
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

### 精简开机自启动服务
注意: 刚装完操作系统一般可以只保留crond，network，syslog，sshd这四个服务。 后期根据业务需求制定自启服务 #（Centos6.x为rsyslog Cetnos5.x为syslog） 如果是中文的话。可能会需要LANG=en 或者替换 3:on 成 3:启用


### 设置系统字符集
```
第一种：vi /etc/sysconfig/i18n
如果想用中文提示：LANG=”zh_CN.UTF-8″ 如果想用英文提示：LANG=”en_US.UTF-8″ 如果临时切换也可以 LANG=zh_CN.UTF-8

第二种：使用sed快速替换
#替换成英文
sed -i 's#LANG="zh_CN.*"#LANG="en_US.UTF-8"#' /etc/sysconfig/i18n
#替换成中文
sed -i 's#LANG="en_US.*"#LANG="zh_CN.UTF-8"#' /etc/sysconfig/i18n
#替换成UTF-8中文
sed -i 's#LANG="zh_CN.*"#LANG="zh_CN.UTF-8"#' /etc/sysconfig/i18n
```

### 清理登陆的时候显示的系统及内核版本
```
#查看登陆信息
cat /etc/redhat-release cat /etc/issue
#清理登陆信息
echo >/etc/redhat-release
echo >/etc/issue
```

### 内核参数优化 vi /etc/sysctl.conf
```
#可用于apache，nginx，squid多种等web应用
net.ipv4.tcp_max_syn_backlog = 65536
net.core.netdev_max_backlog = 32768
net.core.somaxconn = 32768
 
net.core.wmem_default = 8388608
net.core.rmem_default = 8388608
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
 
net.ipv4.tcp_timestamps = 0
net.ipv4.tcp_synack_retries = 2
net.ipv4.tcp_syn_retries = 2
 
net.ipv4.tcp_tw_recycle = 1
#net.ipv4.tcp_tw_len = 1
net.ipv4.tcp_tw_reuse = 1
 
net.ipv4.tcp_mem = 94500000 915000000 927000000
net.ipv4.tcp_max_orphans = 3276800
 
#net.ipv4.tcp_fin_timeout = 30
#net.ipv4.tcp_keepalive_time = 120
net.ipv4.ip_local_port_range = 1024 65535
 
#以下参数是对centos6.x的iptables防火墙的优化，防火墙不开会有提示，可以忽略不理。
#如果是centos5.X需要吧netfilter.nf_conntrack替换成ipv4.netfilter.ip
#centos5.X为net.ipv4.ip_conntrack_max = 25000000
net.nf_conntrack_max = 25000000
net.netfilter.nf_conntrack_max = 25000000
net.netfilter.nf_conntrack_tcp_timeout_established = 180
net.netfilter.nf_conntrack_tcp_timeout_time_wait = 120
net.netfilter.nf_conntrack_tcp_timeout_close_wait = 60
net.netfilter.nf_conntrack_tcp_timeout_fin_wait = 120
```
立即生效 /sbin/sysctl -p centos6.5可能会报错
error: "net.bridge.bridge-nf-call-ip6tables" is an unknown key
error: "net.bridge.bridge-nf-call-iptables" is an unknown key
error: "net.bridge.bridge-nf-call-arptables" is an unknown key
出现这个的原因是，没有自动载入bridge桥接模块

```
modprobe bridge
echo "modprobe bridge">> /etc/rc.local
@查看桥接 
lsmod|grep bridge
```
centos5.X可能会报错 这个错误可能是你的防火墙没有开启或者自动处理可载入的模块ip_conntrack没有自动载入，解决办法有二，一是开启防火墙，二是自动处理开载入的模块ip_conntrack
error: "net.ipv4.ip_conntrack_max"is an unknown key
error: "net.ipv4.netfilter.ip_conntrack_max"is an unknown key
error: "net.ipv4.netfilter.ip_conntrack_tcp_timeout_established"is an unknown key
error: "net.ipv4.netfilter.ip_conntrack_tcp_timeout_time_wait"is an unknown key
error: "net.ipv4.netfilter.ip_conntrack_tcp_timeout_close_wait"is an unknown key
error: "net.ipv4.netfilter.ip_conntrack_tcp_timeout_fin_wait"is an unknown key
centos5.X解决方法：

```
modprobe ip_conntrack
echo "modprobe ip_conntrack">> /etc/rc.local
```
centos6.X可能会报错 这个错误可能是你的防火墙没有开启或者自动处理可载入的模块ip_conntrack没有自动载入，解决办法有二，一是开启防火墙，二是自动处理开载入的模块ip_conntrack
error: "net.nf_conntrack_max"isan unknown key
error: "net.netfilter.nf_conntrack_max"isan unknown key
error: "net.netfilter.nf_conntrack_tcp_timeout_established"isan unknown key
error: "net.netfilter.nf_conntrack_tcp_timeout_time_wait"isan unknown key
error: "net.netfilter.nf_conntrack_tcp_timeout_close_wait"isan unknown key
error: "net.netfilter.nf_conntrack_tcp_timeout_fin_wait"isan unknown key
centos6.X解决方法：

```
modprobe nf_conntrack
echo "modprobe nf_conntrack">> /etc/rc.local
```
注意：笔者在整理这篇centos6.5内核优化的时候发现，如果不开启ip6tables去优化nf_conntrack模块去执行上面的解决方法会依旧提示上面的error。所以在优化服务的时候，可以选择留下iptables和ip6tables。当然如果不用iptables的话，在内核优化的时候就要去掉对nf_conntrack的设置，在进行/sbin/sysctl -p 是不会有错误提示的。

### 如果安装sendmail必须定时自动清理/var/spool/clientmqueue/下文件防止inode节点被占满
```
#centos6.5已经不自动安装sendmail了所以没必要走这一步优化
mkdir -p /server/scripts
vi /server/scripts/spool_clean.sh
#!/bin/sh
find/var/spool/clientmqueue/-typef -mtime +30|xargsrm-f
```

### 删除不必要的系统用户和群组
```
#删除不必要的用户
userdel adm
userdel lp
userdel sync
userdel shutdown
userdel halt
userdel news
userdel uucp
userdel operator
userdel games
userdel gopher
userdel ftp
#删除不必要的群组
groupdel adm
groupdel lp
groupdel news
groupdel uucp
groupdel games
groupdel dip
groupdel pppusers
```

### 关闭重启ctl-alt-delete组合键
```
vi /etc/init/control-alt-delete.conf
#注释掉
#exec /sbin/shutdown -r now "Control-Alt-Deletepressed"
```

### 设置一些全局变量
```
#设置自动退出终端，防止非法关闭ssh客户端造成登录进程过多，可以设置大一些，单位为秒
echo "TMOUT=3600">> /etc/profile
#历史命令记录数量设置为10条
sed -i "s/HISTSIZE=1000/HISTSIZE=10/" /etc/profile
#立即生效
source /etc/profile
```

1.关闭不需要的服务
crond、irqbalance(提升系统性能和降低能耗)、network、sshd、syslog（未列出的全部关闭）
*可选择性的关闭iptables和SELinux

2.关闭不需要的tty（保留2个就可以）
vim /etc/inittab
init -q （不重启生效）

4.修改shell命令的history的记录个数
vim /etc/profile
source /etc/profile

10.关闭centos的写磁盘I/O功能
vim /etc/fstab
追加类似该格式的(实际情况在修改分区)
/dev/sda5 /data/pics ext3 noatime,nodiratime 0 0
11.优化内核参数
12.包的安装（注意尽量源码安装）
