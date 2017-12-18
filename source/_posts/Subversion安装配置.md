---
title: Subversion安装配置
date: 2017-01-17 09:28:59
tags:
- Linux
- Subversion
---
[CentOS 7.2 安装Subversion（SVN）](http://blog.csdn.net/wh211212/article/details/53128805)[centos7.2环境nginx+mysql+php-fpm+svn配置walle自动化部署系统详解](http://blog.csdn.net/reblue520/article/details/52840148)[CentOS下 SVN版本控制的安装（包括yum与非yum）](http://www.linuxsir.org/linuxfxbsuse/12252.html)[centos7使用systemd配置svn开机启动](https://www.centos.bz/2017/08/centos-systemd-svn-auto-boot/)[centos7安装并配置svn](http://www.jianshu.com/p/5f40acfdf8e3)[CentOS 7 设置 svn 开机启动](http://blog.csdn.net/realghost/article/details/52396648)

###更改yum源
```
echo -e '[WandiscoSVN]\nname=Wandisco SVN Repo\nbaseurl=http://opensource.wandisco.com/centos/$releasever/svn-1.9/RPMS/$basearch/\nenabled=1\ngpgcheck=0\n' > /etc/yum.repos.d/wandisco-svn.repo  

yum --disablerepo="*" --enablerepo="WandiscoSVN" install subversion
```

###安装subversion
```
yum install -y subversion
```
###查看subversion版本
```
svnserve --version
```

###创建版本仓库
```
mkdir -p /data/svn/project                                
svnadmin create /data/svn/project/
```
查看/data/svn/project 文件夹发现包含了conf, db,format,hooks, locks, README.txt等文件，说明一个SVN库已经建立。

###配置权限
```
cd /data/svn/project/conf/            //进入配置目录
```
```
vim svnserve.conf                    //编辑配置文件

#匿名访问权限，可以是read，write，none，默认是read
anon-access = no
#使授权用户有写的权限
auth-access = write
#密码数据库的路径
password-db = passwd
#访问控制文件
authz-db = authz
#认证命名空间，subversion会在认证提示里显示，并且作为凭证缓存的关键字
 realm = /data/svn/project
```
```
vim passwd                        //编辑密码文件
user1 = password123
```
```
vim authz

[/]
user1 = rw 
```

### 启动svn
svnserve -d -r /data/svn/              //启动SVN
netstat -ln | grep 3690               //查看端口状态

### 创建源仓库，以“/var/svn/repos/project”为例
```
svn mkdir file:///var/svn/repos/project/trunk -m 
svn mkdir file:///var/svn/repos/project/branches -m "create" # 创建分支
svn mkdir file:///var/svn/repos/project/tags -m "create" # 创建标签 
```

### 导入已存在的代码文件到SVN仓库，导入/home/project目录的文件
```
ll /home/project/
total 0
-rw-r--r-- 1 root root 0 Nov  1 11:57 index.go
-rw-r--r-- 1 root root 0 Nov  1 11:57 index.html
-rw-r--r-- 1 root root 0 Nov  1 11:57 index.php
-rw-r--r-- 1 root root 0 Nov  1 11:58 index.py
-rw-r--r-- 1 root root 0 Nov  1 11:58 info.php

svn import /home/project svn://10.0.0.85/project/trunk -m "initial import"
Adding         /home/project/index.html
Adding         /home/project/index.go
Adding         /home/project/index.php
Adding         /home/project/index.py
Adding         /home/project/info.php
Committed revision 4.

# 确认
svn list svn://10.0.0.85/project/trunk
index.go
index.html
index.php
index.py
info.php
```

启动svnserver，svnserve监听TCP 3690，防火墙开启端口通信
# svn server 端
systemctl start svnserve

#svn client 端
```
yum -y install svn

svn list svn://10.0.0.85/project
```

### 拉取代码到本地
```
svn checkout svn://10.0.0.85/project
```

### SSH方式拉取代码到本地
##如果没有启动svnserve，通过端口无法连接到svn server，可以通过ssh的方式连接到svn server
###svn server 端
```
systemctl stop svnserve
```

# svn client端
```
svn list svn+ssh://root@10.0.0.85/data/svn/project
```

```
vi /var/svn/repos/project/conf/authz
# define groups and users
[groups]
developer = devops,wang
# allow read/write on document root for developer group
[/]
@developer = rw
# allow read on trunk folder for fedora user
[/trunk]
shaon = r 
```

###svn client 客户端测试
```
svn --username shaon list svn://linuxprobe.org/repos/project/trunk
```

#Systemd方式启动

### svnserve.service文件
```
cd /etc/sysconfig
ln -s /data/svn/project/conf/svnserve.conf svnserve.conf
```
```
cat > /usr/lib/systemd/system/svnserve.service<<EOF
[Unit] 
Description=Subversion Server daemon 
After=syslog.target network.target 
[Service] 
Type=forking 
EnvironmentFile=/etc/sysconfig/svnserve
ExecStart=/usr/bin/svnserve --daemon --pid-file=/var/run/svnserve.pid $OPTIONS 

[Install] 
WantedBy=multi-user.target
EOF
```
```
[Unit]:服务的说明
Description:描述服务After:描述服务类别
[Service]服务运行参数的设置
Type=forking是后台运行的形式ExecStart为服务的具体运行命令- ExecReload为重启命令ExecStop为停止命令PrivateTmp=True表示给服务分配独立的临时空间
注意：[Service]的启动、重启、停止命令全部要求使用绝对路径
```

```
cat >/etc/sysconfig/svnserve<<EOF
OPTIONS="-r /data/svn"
EOF
```

###启动
```
#启动svnserve服务
systemctl start svnserve.service

#设置开机自启动
systemctl enable svnserve.service

#停止开机自启动
systemctl disable svnserve.service

#查看服务当前状态
systemctl status svnserve.service

#重新启动服务
systemctl restart svnserve.service

#查看所有已启动的服务
systemctl list-units --type=service
```

#让svn支持http,如果不需要，可以略过。

###安装Apache
`yum install -y httpd mod_dav_svn`

###配置apache

```
vim /etc/httpd/conf.d/subversion.conf
<Location />
  DAV svn
 	SVNParentPath /home/svn
  AuthType Basic
  AuthName "svn repos"
  AuthUserFile /etc/httpd/conf/passwd
  AuthzSVNAccessFile /home/svn/lamp/conf/authz
  Satisfy Any
  Require valid-user
</Location>
```
```
/etc/httpd/conf/passwd文件通过htpasswd -c /etc/httpd/conf/passwd username生成。
```

启动

```
systemctl enable httpd
systemctl start httpd
systemctl status httpd
```