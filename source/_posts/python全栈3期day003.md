01.网络基础和dos命令.mp4 56:43
02.为何学习linux.mp4 08:59
03.课程内容介绍.mp4 03:12
04.操作系统内核与系统调用.mp4 22:18
05.操作系统安装原理.mp4 09：02
06.linux操作系统安装part1.mp4 28：12
07.linux操作系统安装part2.mp4 14:33
08.初识linux命令.mp4 34:49
09.linux操作系统目录结构.mp4 42:19
10.目录及文件操作.mp4 44:05

2016.11.09 09:39
## 01
局域网
城域网
广域网

传输介质
有线
光纤
无线

568A
568B

交叉线
568A->568B

同种设备之间连接用交叉线

非同种设备之间连接用直通线

IP地址
五类

A
B
C
D
E


ipconfig 
ping

cd
d:

rd 删除目录

## 02
Linux学习

## 03

## 04
操作系统核心 内核:内核是一个管理和控制程序,负责管理计算机的所有物理资源,文件系统、内存管理，设备管理，进程管理。
系统调用接口 api

内核态:
用户态:

RHEL、CentOS、Fedora、Ubuntu、SuSE

## 05
Linux系统笔刷安装
硬件配置
1核心 
内存2GB
硬盘20GB

## 06
系统镜像盘 iso

CentOS 6.8

/boot 500MB
swap 虚拟内存,一般内存的1.5或者2倍.大于16GB就16GB了
/ 剩余所有

Minimal 安装

## 07
Kdump

## 08
初识linux命令

UNIX是什么
MINIX
多任务多用户

GNU 和 GPL

Linux是一个基于POSIX和UNIX的多用户、多任务、支持多线程和多CPU的操作系统

常用桌面KDE和GNOME

关闭Linux系统的命令 init 0

登录后的提示符 $
root用户的提示符 #

退出命令 exit

Linux命令的格式

命令 选项 参数

ls -a /

ls

whoami
who
date 
hwclock -s
cal
clear ctrl+l

useradd
su
passwd

终止命令运行 ctrl + c

man
help
--help

## 09
目录

/ 根目录
. 本目录
.. 上一级目录

pwd

/bin 二进制可以执行文件
/sbin 系统管理的二进制可执行文件
/root root用户的家目录
/home 普通用户的默认家目录
/dev 设备文件目录
/etc 配置文件目录

绝对路径和相对路径

绝对路径 必须以一个正斜线"/"开始
相对路径 相对于某个目录

## 10
cp
mv
mkdir
touch
rm
alias
tr
cat
head
tail
more
less




