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

## 修改ip地址、网关、主机名、DNS等
```
vi /etc/sysconfig/network-scripts/ifcfg-eth0

DEVICE=eth0         #网卡名字
BOOTPROTO=static    #静态IP地址获取状态 如：DHCP表示自动获取IP地址
ONBOOT=yes
IPADDR=10.0.0.6
NETMASK=255.255.255.0 
```
```
修改网关
vi /etc/sysconfig/network
GATEWAY=10.0.0.1    #修改默认网关,如果上面eth0里面不配置网关的话，默认就使用这里的网关了。

修改DNS
vi /etc/resolv.conf
nameserver 114.114.114.114
nameserver 8.8.8.8

service network restart   #重启网卡，生效
#重启网卡，也可以用下面的命令
/etc/init.d/network restart
```

## 设置时区
```
rm -f /etc/localtime
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```

## 添加普通用户并进行sudo授权管理
```
useradd sunsky
echo "123456"|passwd --stdin sunsky&&history –c

visudo
在root    ALL=(ALL)    ALL此行下，添加如下内容
sunsky    ALL=(ALL)    ALL
```

## 更改YUM源
```
rm /etc/yum.repos.d/* -rf;curl -ssL http://mirrors.163.com/.help/CentOS6-Base-163.repo>/etc/yum.repos.d/CentOS-Base.repo;echo $?
```

## 安装常用工具
```
yum install -y ntpdate lrzsz wget vim git tree bash-completion sysstat lsof
```

lrzsz是一个上传下载的软件
sysstat是用来检测系统性能及效率的工具

## 定时自动更新服务器时间
```
echo '*/10 * * * * /usr/sbin/ntpdate time.nist.gov >/dev/null 2>&1' >>/var/spool/cron/root
```

## 精简开机自启动服务
刚装完操作系统可以只保留crond，network，rsyslog，sshd这四个服务

```
#关闭所有服务
for sun in `chkconfig --list|grep 3:on|awk '{print $1}'`;do chkconfig --level 3 $sun off;done
#开启需要的服务
for sun in crond rsyslog sshd network;do chkconfig --level 3 $sun on;done
#查看开机启动服务
chkconfig --list|grep 3:on
```

## 变更默认的ssh服务端口，禁止root用户远程连接
```
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak

vim /etc/ssh/sshd_config

Port 52113#ssh连接默认的端口
PermitRootLogin no   #root用户黑客都知道，禁止它远程登录
PermitEmptyPasswords no #禁止空密码登录
UseDNS no            #不使用DNS

/etc/init.d/sshd reload    #从新加载配置
netstat -lnt     #查看端口信息
lsof -i tcp:52113
```

## 锁定关键文件系统
```
[root@c64 ~]# chattr +i /etc/passwd
[root@c64 ~]# chattr +i /etc/inittab
[root@c64 ~]# chattr +i /etc/group
[root@c64 ~]# chattr +i /etc/shadow
[root@c64 ~]# chattr +i /etc/gshadow
使用chattr命令后，为了安全我们需要将其改名
/bin/mv /usr/bin/chattr /usr/bin/任意名称
```

## 文件描述符
文件描述符在形式上是一个非负整数。实际上，它是一个索引值，指向内核为每一个进程所维护的该进程打开文件的记录表。当程序打开一个现有文件或者创建一个新文件时，内核向进程返回一个文件描述符。在程序设计中，一些涉及底层的程序编写往往会围绕着文件描述符展开。但是文件描述符这一概念往往只适用于Unix、Linux这样的操作系统。
习惯上，标准输入（standard input）的文件描述符是 0，标准输出（standard output）是 1，标准错误（standard error）是 2。尽管这种习惯并非Unix内核的特性，但是因为一些 shell 和很多应用程序都使用这种习惯，因此，如果内核不遵循这种习惯的话，很多应用程序将不能使用。

```
#临时修改，立刻生效
ulimit -n 655360         
 
#永久修改
echo "* soft nofile 655360" >> /etc/security/limits.conf&&echo "* hard nofile 655360" >> /etc/security/limits.conf
```

## 调整字符集，使其支持中文
```
sed-i 's#LANG="en_US.UTF-8"#LANG="zh_CN.GB18030"#'/etc/sysconfig/i18n
source/etc/sysconfig/i18n
扩展：什么是字符集？
简单的说就是一套文字符号及其编码。常用的字符集有：
GBK 定长双字节不是国际标准，支持系统不少
UTF-8 非定长 1-4字节广泛支持，MYSQL也使用UTF-8
```

## 去除系统及内核版本登录前的屏幕显示
```bash
>/etc/redhat-release
>/etc/issue
```

## 内核参数优化
说明：本优化适合apache，nginx，squid多种等web应用，特殊的业务也可能需要略作调整。

```
vi /etc/sysctl.conf

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

sysctl –p    #使配置文件生效
```

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

#关闭selinux
```
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config

getenforce
```

### 关闭火墙
```
service iptables stop
iptables -L
service iptables save
```

### 修改文件句柄数  
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

## 修改SSH端口号和屏蔽root账号远程登陆
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

## 关闭不需要的tty（保留2个就可以）
vim /etc/inittab
init -q （不重启生效）

## 修改shell命令的history的记录个数
vim /etc/profile
source /etc/profile

## 关闭centos的写磁盘I/O功能
vim /etc/fstab
追加类似该格式的(实际情况在修改分区)
/dev/sda5 /data/pics ext3 noatime,nodiratime 0 0

## 优化内核参数
## 包的安装（注意尽量源码安装）

## 重启
```
shutdown -r now
```
1