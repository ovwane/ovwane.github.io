---
title: Rsync 配置
date: 2016-12-27 15:55:02
categories:
- 技术
- Linux
tags:
- CentOS
- rsync
---

# [Rsync](http://www.showerlee.com/archives/419)

## 简介
rsync是linux下的文件同步服务，功能简单来说就是服务端打开873端口,客户端连接这个端口,并对服务器端配置的目录进行同步,可以理解为客户端比对服务器端资源后，对增量或者差异的数据进行增删改操作，功能支持上传(推送)或下载（获取）比对，也就是远程数据比对本地数据而后对远程数据进行增删改操作，以及本地数据比对远程数据然后对本地数据进行增删改操作。
    centos6.3下默认已经安装，只需保证依赖服务xinetd开启即可。
    

</more>



## 配置

首先关闭selinux与iptables

```
# vi /etc/sysconfig/selinux
---------
SELINUX=disabled
---------
# setenforce 0
# service iptables stop
```
配置分为2个部分

## server端：
1. 安装rsync(centos6.3默认已安装)

```
# yum install rsync -y
# yum install xinetd -y
```

2. 启动rsync依赖服务

```
# /etc/init.d/xinetd start
# chkconfig xinetd on
3.配置/etc/rsyncd.conf
----------------------
uid = root
gid = root
use chroot = no
max connections = 10
strict modes = yes
port = 873
address = 172.24.40.30
[mail]
path = /home/domains/
comment = mirror for extmail
ignore errors
read only = no
list = no
auth users = user
secrets file = /etc/rsync.pas
hosts allow = 172.24.40.50
hosts deny = 0.0.0.0/0
pid file = /var/run/rsyncd.pid
lock file = /var/run/rsync.lock
log file = /var/log/rsyncd.log
--------------------------
# 表示可写
read only = no
# 端口
port = 873
```

4. 配置/etc/rsync.pas  
在服务器端，必须加入登陆名和密码，在client上，只需要输入密码

```
# vi /etc/rsync.pas
----------------
user:123456
---------------
并给与相关权限(重要,必须是600)
# chmod 600 /etc/rsync.pas
```

5. 配置/etc/rsync.motd

```
# vi /etc/rsyncd.motd
welcome to use the rsync services!
```

6. 启动rsync

```
# rsync --daemon --config=/etc/rsyncd.conf
```

## client端：
1. 安装rsync(centos6.3默认已安装)

```
# yum install rsync -y
# yum install xinetd -y
```

2. 启动rsync依赖服务

```
# /etc/init.d/xinetd start
# chkconfig xinetd on
```

3.客户端必须配置密码文件

```
# vi /etc/rsync.pas
------------------------
123456
----------------------
并给与相关权限(重要,必须是600)
# chmod 600 /etc/rsync.pas
```

4. 然后在客户端输入命令同步：

```
下载(获取):
# rsync -auzv --progress --delete  --password-file=/etc/rsync.pas user@172.24.40.30::mail      /home/domains
上传(推送):
# rsync -auzv --progress --delete  --password-file=/etc/rsync.pas /home/domains/* user@172.24.40.30::mail     
```
```
注：后面加 --port 873 可添加端口号信息，若在配置文件中自定义端口，这里需要加--port参数。
请注意这部分:
user@172.24.40.30::mail /home/domains
----------------
user对应server端配置文件的auth users = user
      和server端/etc/rsync.pas内用户名:密码(user:123456)
172.24.40.30对应服务端IP地址
mail对应server端配置文件的[mail]
/home/domains表示client端同步server端数据后数据保存在client端目录的路径
@与::均起到字符连接功能
----------------
```
以上方法为明文传输，传输过程中密码可能会被sniffer截取：
所以推荐线上使用ssh秘钥认证+rsync加密传输，
[ssh秘钥认证配置](http://showerlee.blog.51cto.com/2047005/1217651)

按照上面的格式加密重写(完整格式)：

```
下载(获取):
# rsync -auzvP --delete -e "ssh -p22" root@172.24.40.30:/home/domains/ /home/domains/ --port 873
上传(推送):
# rsync -auzvP --delete -e "ssh -p22" /home/domains/ root@172.24.40.30:/home/domains/ --port 873
```
注： --progress 可以简写为 -P

5. 定时计划任务：      
在crontab中增加一条命令，设置每分钟自动执行一次。

```
# crontab -e
----------------------
* * * * * /usr/bin/rsync  -auzv --progress --delete  --password-file=/etc/rsync.pas user@172.24.40.30::mail /home/domains
---------------------
```

6. 为了安全，若系统启用防火墙，建议增加一条iptables命令

```
# iptables -A INPUT -P tcp --dport:873 -j ACCEPT
```

7. 如果需要多个不同目录支持同步，再增加一个

```
[MYSQL]
path = /usr/local/mysql/data
comment = mysql mirror
ignore errors
read only = yes
list = no
auth users = root
secrets file = /etc/rsync.pas
hosts allow = 172.24.40.50
hosts deny = 0.0.0.0/0
pid file = /var/run/rsyncd.pid
lock file = /var/run/rsync.lock
log file = /var/log/rsyncd.log
```

重启rsync

```
# pkill rsync
# rsync --daemon --config=/etc/rsyncd.conf
```

然后在客户端输入命令同步

```
rsync  -auzv --progress --delete  --password-file=/etc/rsync.pas user@172.24.40.30::MYSQL /usr/local/mysql/data
```



##问题

注：配置过程问题：

问题1：

```
在client上遇到问题：
rsync -auzv --progress --password-file=/etc/rsync.pas root@192.168.133.128::backup /home/
rsync: could not open password file "/etc/rsync.pas": No such file or directory (2)
Password:
@ERROR: auth failed on module backup
rsync error: error starting client-server protocol (code 5) at main.c(1506) [Receiver=3.0.7]
遇到这个问题：client端没有设置/etc/rsync.pas这个文件，而在使用rsync命令的时候，加了这个参数--
password-file=/etc/rsync.pas
```

问题2：

```
rsync -auzv --progress --password-file=/etc/rsync.pas root@192.168.133.128::backup /home/
@ERROR: auth failed on module backup
rsync error: error starting client-server protocol (code 5) at main.c(1506) [Receiver=3.0.7]
遇到这个问题：client端已经设置/etc/rsync.pas这个文件，里面也设置了密码111111，和服务器一致，但是
服务器段设置有错误，服务器端应该设置/etc/rsync.pas  ，里面内容root:111111 ,这里登陆名不可缺少
```

问题3：

```
rsync -auzv --progress --password-file=/etc/rsync.pas root@192.168.133.128::backup /home/
@ERROR: chdir failed
rsync error: error starting client-server protocol (code 5) at main.c(1506) [Receiver=3.0.7]
遇到这个问题，是因为服务器端的/home/backup  其中backup这个目录并没有设置，所以提示：chdir failed
```

问题4：

```
rsync: write failed on "/home/backup2010/wensong": No space left on device (28)
rsync error: error in file IO (code 11) at receiver.c(302) [receiver=3.0.7]
rsync: connection unexpectedly closed (2721 bytes received so far) [generator]
rsync error: error in rsync protocol data stream (code 12) at io.c(601) [generator=3.0.7]
磁盘空间不够，所以无法操作。
可以通过df /home/backup2010 来查看可用空间和已用空间
```

问题5：网络收集问题

```
1、权限问题
类似如下的提示：rsync: opendir "/kexue" (in dtsChannel) failed: Permission denied (13)注意查看同步的目录权限是否为755
2、time out
rsync: failed to connect to 203.100.192.66: Connection timed out (110)
rsync error: error in socket IO (code 10) at clientserver.c(124) [receiver=3.0.5]
检查服务器的端口netstat –tunlp，远程telnet测试。
3、服务未启动
rsync: failed to connect to 10.10.10.170: Connection refused (111)
rsync error: error in socket IO (code 10) at clientserver.c(124) [receiver=3.0.5]
启动服务：rsync --daemon --config=/etc/rsyncd.conf
4、磁盘空间满
rsync: recv_generator: mkdir "/teacherclubBackup/rsync……" failed: No space left on device (28)
*** Skipping any contents from this failed directory ***
5、Ctrl+C或者大量文件
rsync error: received SIGINT, SIGTERM, or SIGHUP (code 20) at rsync.c(544) [receiver=3.0.5]
rsync error: received SIGINT, SIGTERM, or SIGHUP (code 20) at rsync.c(544) [generator=3.0.5]
6、xnetid启动
rsync: read error: Connection reset by peer (104)
rsync error: error in rsync protocol data stream (code 12) at io.c(759) [receiver=3.0.5]
```

查看rsync日志
rsync: unable to open configuration file "/etc/rsyncd.conf": No such file or directory
xnetid查找的配置文件位置默认是/etc下，根据具体情况创建软链接。例如：
ln -s /etc/rsyncd/rsyncd.conf /etc/rsyncd.conf
或者更改指定默认的配置文件路径，在/etc/xinetd.d/rsync配置文件中



## 使用

使用 ssh 同步文件到本地

```bash
rsync -P --bwlimit=300 -e 'ssh -p 22 -i ./key' root@servername:/path/filename /var/www/local_dir
```

> --bwlimit=300 限速。