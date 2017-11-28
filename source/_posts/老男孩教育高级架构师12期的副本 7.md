---
title: 老男孩教育高级架构师12期
date: 2017-11-18 08:30:20
---
老男孩教育高级架构师12期
日期：2015-10-23 19:53

# L001-老男孩高级架构师12期-zabbix深度实践-13节
## 01-老男孩12期架构师开课动员.flv
年薪30万

公司加班请假

预习作业和课后作业

学习是自己要学的
作业就是工作中的总结

18k
35w
16k
40w
18k
15k
23k
28w

## 02-学习环境统一提升学习效率.avi
上课的效率，很重要。
>先把路跑通，下课再回去变通。
>每节课结束后做快照
>为每个课程创建快照

1CPU
1GB
20GB
cdrom CentOS-6.7-x86_64-bin-DVD1.iso
nat、bridge
eth0
nat
no dhcp
10.0.0.0
255.255.255.0
10.0.0.2 我的配置10.0.0.1

eth1
lan segment 我的配置Host-only
172.16.1.0
255.255.255.0

CentOS6.7_arc_moban(10.0.0.6)

root 123456

模版机 10.0.0.6 172.16.1.6 其他的虚拟机是链接克隆
linux_node1 10.0.0.7 172.16.1.7
linux_node2 10.0.0.8 172.16.1.8
linux_node3 10.0.0.9 172.16.1.9

系统安装的软件包
Minimal
自定义 Base System{Base,Compatibility librayies,Debugging Tools},Development{Development tools}

硬盘分区
/boot 200
swap 768
/

## IP、DNS、网关
```
vi /etc/sysconfig/network-scripts/ifcfg-eth0
ONBOOT=yes
IPADDR=10.0.0.6
NETMASK=255.255.255.0

vi /etc/sysconfig/network
GATEWAY=10.0.0.1

vi /etc/resolv.conf
nameserver 114.114.114.114
```
```
/etc/init.d/network restart
ifoconfig
```
```
>/etc/udev/rules.d/70-persistent-net.rules

vi /etc/sysconfig/network-scripts/ifcfg-eth0
删除
HWADDR：
UUID：
```

## 关闭SELinux
```
setenforce 0&&sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
```

## 关闭 iptables防火墙
```
/etc/init.d/iptables stop&&/etc/init.d/ip6tables stop&& chkconfig iptables off
```

## 禁用ipv6    
```
cat >> /etc/modprobe.d/ipv6.conf <<EOF
alias net-pf-10 off
alias ipv6 off
EOF
```

## 时间同步
```
#更改时区
cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
#立刻同步时间
/usr/sbin/ntpdate 0.asia.pool.ntp.org
#定时任务同步时间
echo '*/5 * * * * /usr/sbin/ntpdate 0.asia.pool.ntp.org >/dev/null 2>&1'>>/var/spool/cron/root
#查看任务状态
crontab -l
```

## 更改YUM源
```
rm /etc/yum.repos.d/* -rf;curl -ssL http://mirrors.163.com/.help/CentOS6-Base-163.repo>/etc/yum.repos.d/CentOS-Base.repo;echo $?
#EPEL
rpm -Uvh http://mirrors.ustc.edu.cn/fedora/epel/6/x86_64/epel-release-6-8.noarch.rpm
#或者使用命令安装，一般推荐用ustc的源。
yum -y install epel-release
```

```
yum -y install tree nmap sysstat lrzsz dos2unix telnet
```
```
yum -y install vim git bash-completion lsof
```

## 精简开机启动项的服务
```bash
for service in `chkconfig --list|grep "3:on"|awk '{print $1}'|grep -vE "crond|network|rsyslog|sshd|sysstat"`;do chkconfig $service off;done
```

## Vim安装配置

## SSH设置
```
vim /etc/ssh/sshd_config
PermitEmptyPasswords no
UseDNS no
GSSAPIAuthentication no
```
```
/etc/init.d/sshd reload
```

## 字符集(没设置）
```
cp /etc/syscofig/i18n /etc/sysconfig/i18n.ori
echo 'LANG="zh_CN.UTF-8"'>/etc/sysconfig/i18n
source /etc/syscofig/i18n
echo $LANG
```

## 文件描述符
```
#查看文件描述符大小
ulimit –n
#
echo '*               -    nofile             65535'>>/etc/security/limits.conf
```

写一段自我介绍


认为的技术
同学你们的圈子
我和我们的老师

## 03-老男孩高级架构师培训体系说明.flv
课程讲的是运维知识体系v0.4
[运维知识体系V2.0-赵班长](https://www.unixhot.com/page/ops)

>学习方法
>盖房子
>滚雪球

盖房子，知道学什么。
滚雪球，不知道学什么，见到什么学什么。

运维经理应该掌握的知识。

必须知道你运行的命令的确切意思。


# 步骤
技术简介
安装部署
运行维护

## 04-网络监控.flv
TCP 十一种状态监控
TIME_WAIT 数量

Web服务监控
链路延时
200OK

nmon
监控宝、基调、听云、博瑞
tool.chinaz.com

DNS解析

smokeping 使用RRDtool绘图 FreeBSD 安装速度快

上海市网络状况
somkeping检测

运维总监

## 05-系统监控.flv
云计算与自动化运维实践
2008年开始运维
赵班长

saltstack.cn
unixhot.com
[赵班长Gituhb](http://github.com/unixhot)

版权声明

## 监控概述：
硬件监控
系统监控
网络监控
应用监控
引入Zabbix
使用Zabbix
自动化监控
流量分析
监控可视化

硬件监控
	IPMI（Intelligent Platform Management Interface）即智能平台管理接口
	[使用 ipmitool 实现 Linux 系统下对服务器的 ipmi 管理](https://www.ibm.com/developerworks/cn/linux/l-ipmi/)
	[IPMI总结](http://www.chenshake.com/summary-of-ipmi/)
	
```
yum -y install ipmitool
```

IPMI查看不了硬盘状态
MegaCli工具查看Raid磁盘阵列状态

了解监控对象，性能的基准线。什么是好，什么是坏。

如何来做。

根据业务需求来做的。

# 查看CPU信息的工具
cat /proc/cpuinfo
lscpu

# CPU的使用率
top

内核态 30%
用户态 70%

计算密集型
IO密集型

上下文切换

1核 3，2核6。


# 系统负载
cat /proc/loadavg
uptime

内存使用率

## 06-应用监控.flv
ps aux|grep flush

apache模块 mod_status

nginx模块 http_status 

memcached `telnet IP 11211`

redis 

```
redis-cli 
info
```

面向运维的开发
DevOps

做需求分析
怎么监控
怎么做安全
怎么做ACL
验证

这么多监控需求，自己写脚本不是要累死吗、所以引入了监控工具。

## 07-引入zabbix.flv
2009 年 nagios 监控报警
cacti 图形比较好看

2010 转向 zabbix

### Zabbix
[Zabbix](http://www.zabbix.com)

Zabbix Server 编译安装

Zabbix 2.4

>怎么在官网找到安装方法

[Zabbix Documentation 2.4](https://www.zabbix.com/documentation/2.4/manual/installation/install_from_packages#red_hat_enterprise_linuxcentos)

### zabbix 服务端
`
rpm -ivh http://repo.zabbix.com/zabbix/2.4/rhel/6/x86_64/zabbix-release-2.4-1.el6.noarch.rpm
`
`rpm -ql zabbix-release`


```
zabbix
zabbix-server
zabbix-web
zabbix-server-mysql
zabbix-web-mysql
zabbix-agent
zabbix-get
```
```
yum -y install zabbix zabbix-server zabbix-web zabbix-server-mysql zabbix-web-mysql zabbix-agent zabbix-get
```

## 数据库
`yum install -y mysql-server mysql`

`cp /usr/share/mysql/my-medium.cnf /etc/my.cnf`
```
vim /etc/my.cnf

[mysqld]
character-set-server = utf8
init-connect = 'SET NAMES utf8'
collation-server = utf8_general_ci
```

```
/etc/init.d/mysqld start
chkconfig mysqld on
```
```
#修改mysql数据库root密码
/usr/bin/mysqladmin -u root password 'new-password'

/usr/bin/mysql_secure_installation
```


创建数据库

```
create database zabbix character set utf8 collate utf8_bin;
grant all privileges on zabbix.* to zabbix@localhost identified by 'zabbix';
quit;
```

导入zabbix数据库模版,按顺序导入。

```
cd /usr/share/doc/zabbix-server-mysql-2.4.8/create

mysql -uroot -p zabbix < schema.sql
mysql -uroot -p zabbix < images.sql
mysql -uroot -p zabbix < data.sql
```

更改时区

```
sed -i 's@# php_value date.timezone Europe/Riga@php_value date.timezone Asia/Shanghai@g' /etc/httpd/conf.d/zabbix.conf
```
```
/etc/init.d/httpd start
chkconfig httpd on
```

Zabbix-server 配置 mysql 数据库ip

```
vim /etc/zabbix/zabbix_server.conf

DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=zabbix
DBPort=3306
```
```
service zabbix-server start
chkconfig zabbix-server on

lsof -i:10050
```

## 08-zabbix简介.flv
# zabbix-web配置
http://IP/zabbix
用户名：Admin
密码：zabbix

"/etc/zabbix/web/zabbix.conf.php"

第一步改密码

#zabbix 客户端配置
`
rpm -ivh http://repo.zabbix.com/zabbix/2.4/rhel/6/x86_64/zabbix-release-2.4-1.el6.noarch.rpm
`
```
yum -y install zabbix-agent
```
```
vim /etc/zabbix/zabbix_agentd.conf

#被动模式
Server=10.0.0.7
Hostname=Zabbix server
```
```
vim /etc/zabbix/zabbix_agentd.conf

#主动模式
StartAgents=0
ServerActive=10.0.0.7
Hostname=linux_node2.ovwane.com
```
```
/etc/init.d/zabbix-agent start
chkconfig zabbix-agent on
```

## 09-zabbix自定义监控和图表.flv
自定义监控项
1、改配置文件
```
vim /etc/zabbix/zabbix_agentd.conf

#给zabbix传递数据最大512kb。
UserParameter=login-user,uptime|awk '{print $4}'

#重启zabbix-agent
/etc/init.d/zabbix-agent
```

服务端获取数据

```
zabbix_get -s 10.0.0.8 -k login-user
```

2、在web界面添加

Item
Configuration->Hosts->linux-node2->Items-Create item

图形
node2->Grahp-Create Grahp

## 10-流量分析.flv
IP 并发1万，PV 20万，

开源分析类似于Google统计和百度统计

[PIWIK](http://www.piwik.org)

写活动的运维报告

活动趋势

访问量
流量
趋势

写报告给BOSS。

网站的PV多少，访问量多少。促进了多少交易。保证了多少业务的稳定运行。

## 11-zabbix自定义报警-动作.flv

Media Types->自定义报警脚本
```
/etc/zabbix/za bix_server.conf
AlertScriptsPath=/usr/lib/zabbix/alertscripts
```

pymail.py

企信通

短信

责任心

>在其位，谋其事。

监控宝


```
/etc/zabbix/zabbix_agentd.d/userparameter_mysql.conf
```

## 12-zabbix自定义模板.flv

```
cd /etc/zabbix/shells/
vim zabbix_linux_plugin.sh

cd /etc/zabbix/zabbix_agentd.d/
vim zabbix_linux_plugin.conf
UserParameter=linux_status[*],/etc/zabbix/shells/zabbix_linux_plugin.sh "$1" "$2" "$3"
```
```
zabbix_get -s 10.0.0.8 -k linux[tcp_status,ESTAB]
```

导入模版

# 没有讲的内容
zabbix监控snmp监控交换机
zabbix分布式 zabbix-proxy
zabbix自动化监控

## 13-监控可视化.mp4_OK.flv
自动化监控

Action：
网络发现
自动发现
ZabbixAPI
	cmbd

数据可视化
监控的目标是保证业务的稳定

业务波动不能体现。业务监控加进来

业务访问量、注册用户量、订单量

自定义key
研发业务逻辑封装好，只用调用参数。

腾讯蓝鲸
小米flacon

关系数、业务关系数。

关系数
[百度关系数](http://echarts.baidu.com)

业务监控
BI商业智能，提供监控。

安全监控 Nginx+lua+waf
日志监控 ELK

舆情监控

# 监控体系
触发器，
监控项是不是要设置触发器。

3个月，1年 日志保存期。

# L002-老男孩高级架构师12期-zabbix深度实践2-2节
## 1-Zabbix分布式监控.flv
查官网文档安装配置。

>先百度google搜索解决问题
>在去官方查看文档

1万个监控主机
zabbix_agent 主动模式、被动模式
被动模式：
	只监听端口，server请求数据
	Server
	
主动模式：
	ServerActive
	
zabbix分布式监控zabbix proxy

只是数据的收集器、不会报警，计算。

## 安装Zabbix Proxy 服务器
`
rpm -ivh http://repo.zabbix.com/zabbix/2.4/rhel/6/x86_64/zabbix-release-2.4-1.el6.noarch.rpm
`
```
yum install -y zabbix-proxy zabbix-proxy-mysql mysql-server
```

数据库配置

>学的不是步骤，而是方法、学的是方法。

创建用户和数据库，并导入数据

```
#导入数据库模版
use zabbix;
source /usr/share/doc/zabbix-rpoxy-mysql-*/create/schema.sql;

zabbix_r 只读用户名
zabbix_rw 读写用户名
```

```
vim /etc/zabbix/zabbix-proxy.conf

ProxyMode=0
Server=10.0.0.7
Hostname=proxy-node1.ovwane.com
DBname=
DBuser=
DBPassword=
```

```
/etc/init.d/zabbix-proxy start
chkconfig zabbix-proxy on
```

## Zabbix Web 设置proxy-node1
Administration->Proxies->

主机添加
proxy监听

Zabbix-agent 添加ServerActive为proxy-node1的IP地址。

# 2-Zabbox自动化监控.flv
Zabbix Agent 自动注册
自动发现
Zabbix API

## Zabbix Agent自动注册方式
```
vim /etc/zabbix/zabbix_agent.conf
# HostMetadata=
HostMetadataItem=system.uname
```

zabbix事件（自动注册是事件）

配置一个Active。
源自 自动注册

条件

## 自动发现

```
vim zabbix_agent.conf
Server=10.0.0.7
StartAgent=3
```

## Zabbix API
发送 post请求
api_jsonrpc.php

1、验证
获取sessionid，以后请求就可以用sessionid请求就可以了。

```
curl -s -X POST -H 'Content-Type:application/json' -d '
{
    "jsonrpc": "2.0",
    "method": "user.login",
    "params": {
        "user": "Admin",
        "password": "zabbix"
    },
    "id": 1
}' http://10.0.0.7/zabbix/api_jsonrpc.php|python -mjson.tool
```

```
{"jsonrpc":"2.0","result":"f5e66c8fde0e7f4a9e38cd459434740e","id":1}
```

```
curl -s -X POST -H 'Content-Type:application/json' -d '
{
    "jsonrpc": "2.0",
    "method": "host.get",
    "params": {
        "output": ["hostid"],
        "selectGroups": "extend",
        "filter": {
            "host": [
                "Zabbix server"
            ]
        }
    },
    "auth": "f5e66c8fde0e7f4a9e38cd459434740e",
    "id": 2
}' http://10.0.0.7/zabbix/api_jsonrpc.php | python -mjson.tool
```

```
{"jsonrpc":"2.0","result":[{"hostid":"10084","groups":[{"groupid":"4","name":"Zabbix servers","internal":"0","flags":"0"}]}],"id":2}
```

1、验证
2、请求API，附带上SessionID

```
curl -s -X POST -H 'Content-Type:application/json' -d '
{
    "jsonrpc": "2.0",
    "method": "host.get",
    "params": {
        "output": ["hostid"],
        "selectGroups": "extend",
        "filter": {
            "host": [
                "Zabbix server"
            ]
        }
    },
    "auth": "038e1d7b1735c6a5436ee9eae095879e",
    "id": 2
} ' http://10.0.0.7/zabbix/api_jsonrpc.php|python -mjson.tool
```

CMBD 一定是唯一的。

Zabbix API 好用。

Zabbix API python模块。


