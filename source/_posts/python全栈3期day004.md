01.上节课复习.mp4 17：25
02.创建用户相关的文件.mp4 55：10
03.用户增删该查及组相关操作.mp4 26：05
04.对文件的权限管理.mp4 1：08：24
05.对目录的权限管理.mp4 26：08
06.权限管理补充..mp4 04:10
07.属主属组及基于数字的权限管理.mp4 15：33

开始日期 2016.11.10 09：40
2016.11.10 16:30

## 01
怎么安装系统

系统提示符$和#的区别

[Linux基础](http://www.cnblogs.com/linhaifeng/articles/6045600.html)

相对路径和绝对路径


## 02
linux核心一切皆文件

useradd 
/etc/passwd

用户:密码:uid:gid:描述信息:用户家目录:sh类型
root:x:0:0:root:/root:/bin/bash

/etc/shadow


/etc/group

组名:密码:gid
root:x:0

/etc/gshadow

/home 
home下的文件模板 /etc/skel目录下

/var/spool/mail/

id

## 03
useradd 
-u 指定用户的UID
-g 指定用户所属的群组
-d 指定用户目录
-c 指定备注信息
-s 指定shell

userdel

usermod
-d 更改用户家目录
-aG 添加附加组

groupadd

## 04

文件权限信息 硬链接 属主 属组 文件大小 文件创建日期 文件名
-rwxrwxrwx@ 1 jinlong  staff   66347968  3 21 13:54 01.上节课复习.mp4

SELinux

- 文件
d 目录
l 软连接
b 设备
p 管道文件

rwx rwx rwx
属主 属组 其它


chmod
u=user
g=group
o=other 
a=all

执行sh文件 
sh 文件名
bash 文件名
. 文件名
./文件名


## 05

目录可读可写

rwx r可以ls目录下的文件名 w可以新建删除重命名目录内的文件 x可以cd进入目录


## 06
vim 强制保存退出
wq!

## 07
r 读 4,w 写 2,x 执行 1

chown

chown root.root
chown root:root

chown root
chown .root 

touch {1..9}.txt

chmod 664 a.txt



