---
title: Linux就该这么学第五期培训笔记
date: 2016-07-04 19:03:50
categories:
- 技术
- Linux
tags:
- Linux
- RHEL7
- sshd
- httpd
- vsftpd
- Samba
- NFS
- Bind
- DHCP
- postfix
- dovecot
- Squid
- iSCSI
- MariaDB
- PXE
- kickstart
- Git
- OpenStack
- OpenLDAP
---

[Linux就该这么学](http://www.linuxprobe.com/chapter-00.html)[软件资源库](http://www.linuxprobe.com/tools)
>技术是用来为人服务，而不人限制于这个技术必须要学会它，要学就要学有用的东西。

## 第一节课
### 红帽认证 [Red Hat Certified](http://www.linuxprobe.com/redhat-certificate)
RHCSA(Red Hat Certified System Administor)
针对系统的管理

RHCE(Red Hat Certified Engineer)
针对服务的管理

RHCA(Red Hat Certified Arch)
针对红帽自家产品的管理

推荐学习的RHCA方向：
401、436、318、413、442
安全、调优、openstack、储存

### 稻盛和夫的书《活法》
>“工作马马虎虎，只想在兴趣和游戏中寻觅快活，充其量只能获得一时的快感，绝不能尝到从心底涌出的惊喜和快乐，但来自工作的喜悦并不想糖果那样--放进嘴里就甜味十足，而是需要从苦劳与艰辛中渗出，因此当我们聚精会神，孜孜不倦，客服艰辛后的成就感，世上没有哪种喜悦可以类比。”

>“更何况人类生活中工作占据了较大的比重，如果不能从劳动中、工作中获得充实感，那么即使从别的地方找到快乐，最终我们仍然会感到空虚和缺憾。”

### 开源共享精神-常见开源协议
#### GPL
GPL(GNU General Public License)
复制自由：
传播自由：
收费自由：
修改自由：

#### BSD
BSD(Berkeley Software Distribution)

#### Apache
Apache(Apache Licence Version)

#### MPL
MPL(The Mozilla Public License)

#### MIT
MIT(Massachusetts Instiute of Technology)
目前限制最少的开源协议之一，只需要程序的开发者在修改后的源代码中保留原作者的许可信息，所以比较适合于商业软件。 

### 为什么学习Linux系统？
1991年10月份由芬兰赫尔辛基大学的在校生Linus Torvalds编写了一款叫做Linux的操作系统内核。
1995年1月份由Bob Young依据Linux内核，并集成了众多的源代码和程序软件，创办了RedHat公司并开始出售技术服务。

### 常见的Linux发行版
RHEL(Red Hat Enterprise Linux)
CentOS(Community Enterprise Operating System)
Fedora

Debian
Ubuntu

openSUSE

Gentoo

## 第二节课
### VMware Workstation 12
VMware Workstation 12 安装 RHEL7.0

### [重置root用户密码](http://www.linuxprobe.com/chapter-01.html#14_root)
进入boot引导界面

按e键编辑引导信息

查找linux16开头的行并在行尾添加”rd.break“参数

按crtl + x键

依次输入以下命令

```bash
mount -o remount,rw /sysroot
chroot /sysroot
echo "新的root密码" | passwd --stdin root
touch /.autorelabel
exit
reboot
```

### 红帽软件包管理器
RPM(Redhat Package Manager)

```bash
#安装软件
rpm -ivh filename.rpm
#升级软件
rpm -Uvh filename.rpm
#卸载软件
rpm -e filename.rpm
#查询软件的描述信息
rpm -qpi filename.rpm
#列出软件的文件信息
rpm -qpl filename.rpm
#查询文件属于哪个RPM
rpm -qf filename
```

### Yum软件仓库
yum仓库配置文件均须以.repo结尾并存放在/etc/yum.repos.d/目录中。

```
/etc/yum.repos.d/rhel.repo

[rhel-media]:yum源的名称，可自定义。
name=rhel7:yum仓库的名称，可自定义。
baseurl=:源的地址提供（ftp、http、file三种方式）
enable=1: 设置此源是否可用，1为可用，0为禁用。
gpgcheck=1:设置此源是否校检文件，1为校检，0为不校检。
gpgkey=:若为校检文件请指定公钥文件地址。
```
```bash
#更新
yum update
#清理缓存
yum clean all
```

### Systemd初始化进程
#### init和systemd运行级别对比
|Sysvinit运行级别|Systemd目标名称|作用|
|:|
|0|runlevel0.target, poweroff.target|关机|
|1|runlevel1.taget, rescue.target|单用户模式|
|2|runlevel2.target, multi-user.target|等同于级别3|
|3|runlevel3.target, multi-user.target|多用户的文本界面|
|4|runlevel4.target, multi-user.target|等同于级别3|
|5|runlevel5.target, graphical.target|多用户的图形界面|
|6|runlevel6.target, reboot.target|重启|
|emergency|emergency.target|紧急shell|

#### 查看运行目标
```bash
#查看运行目标
runlevel
#更改默认运行目标，不会立即更改运行目标，重启后生效。
systemctl set-default multi-user.target
systemctl set-default graphical.target
#立即更改运行级别重启后失效
systemctl isolate multi-user.target
```

### Shell
sh
bash(Bourne-Again Shell)
zsh
csh

### 执行查看帮助命令
#### man
命令名称 命令参数 命令对象
命令和参数和对象之间必须有空格分割
长格式 man --help
短格式 man -h

man命令的可用帮助文档分类有：

|代码|代表内容|
|:|
|1|普通的命令|
|2|内核调用的函数与工具|
|3|常见的函数与函数库|
|4|设备文件的说明|
|5|配置文件|
|6|游戏|
|7|惯例与协议|
|8|管理员可用的命令|
|9|内核相关的文件|

帮助文档的目录结构与操作方法

|结构名称|代表意义|
|:|
|NAME|命令的名称|
|SYNOPSIS|参数的大致使用方法|
|DESCRIPTION|介绍说明|
|EXAMPLES|演示（附带简单说明）|
|OVERVIEW|概述|
|DEFAULTS|默认的功能|
|OPTIONS|具体的可用选项（带介绍）|
|ENVIRONMENT|环境变量|
|FILES|用到的文件|
|SEE ALSO|相关的资料|
|HISTORY|维护历史与联系方式|

man命令的操作按键

|按键|用处|
|:|
|空格键|向下翻一页|
|[Page Down]|向下翻一页|
|[Page Up]|向上翻一页|
|[HOME]|直接前往首页|
|[END]|直接前往尾页|
|/关键字|从上至下搜索某个关键字|
|?关键字|从下至上搜索某个关键字|
|n|定位到下一个搜索到的关键字|
|N|定位到上一个搜索到的关键字|
|q|退出帮助文档|

## 第三节课
### 常用系统工作命令
#### echo
```bash
echo "字符串"
echo $SHELL
```

#### date
```bash
#格式化时间 %Y年%m月%d日 %H时%M分%S秒
date "+%Y-%m-%d %H:%M:%S"
#显示今天是今年的第几天
date "+%j"
```

#### reboot
重启

#### halt
#### poweroff
关机

#### shutdown
```bash
#重启
shutdown -r now
#关机
shutdown -h now
```

#### wget
```bash
#指定下载文件目录
wget -O /path/to/file
#断点续传
wget -c
```

#### ps
```bash
ps -aux
ps -ef
```

#### top
```
top - 22:45:42 up  1:20,  1 user,  load average: 0.00, 0.01, 0.01
Tasks: 123 total,   2 running, 121 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  0.0 sy,  0.0 ni, 99.9 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  3877776 total,  3598220 free,   121956 used,   157600 buff/cache
KiB Swap:  4063228 total,  4063228 free,        0 used.  3537092 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
    1 root      20   0  193700   6836   4056 S   0.0  0.2   0:01.21 systemd
    2 root      20   0       0      0      0 S   0.0  0.0   0:00.00 kthreadd 
```

#### pidof
```bash
pidof sshd
```

#### pgrep
```bash
pgrep sshd
```

#### kill
```bash
kill -9
```

#### killall
```bash
killall sshd
```

#### jobs
#### fg

### 系统状态检测命令
#### ifconfig ip
#### uname
```bash
uname -a
```

#### uptime
#### free
```bash
free -h
free -m
```

#### who
#### w
#### id
#### last
#### history
```
#清除历史记录，必须退出终端才会清理文件中的内容。
history -c
#！加上历史记录的数字号，执行这个数字后的命令。
!22
```

#### sosreport 只用RHEL有这个命令

### 工作目录切换命令
#### pwd
#### cd
```bash
#切换到用户家目录
cd ~
#切换到上一次的目录
cd -
```

#### ls

### 文本文件编辑命令
#### cat
#### more
#### head
#### tail
#### od 转换文本的字符编码
``` bash
#显示二进制可执行文件的十六进制
od -t x /bin/ls
```

#### tr
```bash
#所有的a-z的小写字母转换为A-Z的大写字母
tr [a-z] [A-Z]
```

#### wc
#### stat

#### cut
```bash
#-d 分隔符：，-f 1,2 第一列和第二列
cut -d : -f 1,2 /etc/passwd
```

#### diff

## 第四节课
### 文件目录管理命令
#### touch
Linux文件中有三种时间
修改时间(mtime)：内容修改时间（不包括权限的）
更改时间(ctime)：更改权限与属性的时间
读取时间(atime)：读取文件内容的时间

```bash
#修改时间修改为2天前（伪造了自己没有动过该文件的假象）
touch -d "2 days ago" test
#仅修改访问时间
touch -a
#仅修改修改时间
touch -m
#同时修改atime和mtime
touch -d
#要修改成的时间[YYMMDDhhmm]，例子[201710111000]
touch -t
```

#### mkdir
```bash
#创建嵌套式目录，例 /root/a/b/c,默认没有a目录和b目录。创建c目录。
mkdir -p /root/a/b/c
#显示创建的过程
mkdir -v
#默认的文件目录权限
mkdir -m 755
```

#### cp
```bash
#复制目录
cp -r
#保留原有的文件权限
cp -a
```

#### mv
#### rm
#### dd
```bash
dd if=/dev/zero of=/root/test bs=100M count=1
```

#### file
```bash
file /bin/ls
```

### 打包压缩与搜索命令
#### tar
```bash
#打包目录，排除指定文件。--exclude
tar czvf ~/etc.tar.gz /etc/* --exclude=/etc/resolv.conf
#解压缩
tar xzvf etc.tar.gz
```

#### grep
```bash
#选择参数之外的选项
grep -v 
#显示行号
grep -n
```

#### find
```bash
#在整个的文件系统中找出所有归属于linuxprobe用户的文件并复制到/root/findresults目录
find / -user linuxprobe -exec cp -a {} /root/findresults/ \;
```

### 管道符、重定向与环境变量
#### 管道命令符
"|"管道命令符

```bash
#查看passwd文件并经过管道作为grep的搜索对象。
cat /etc/passwd | grep 'root'
#使用非交互式设置用户密码
echo "密码" | passwd --stdin root
```

#### 重定向
|代码|类型|
|:|
|0|标准输入STDIN|
|1|标准输出STDOUT|
|2|标准错误输出STDERR|

标准输出重定向
">"清空输出，覆盖文件内容。
">>"追加输出，不覆盖文件内容。

标准错误输出重定向
"2>"清空输出，覆盖文件内容。
"2>>"追加输出，不覆盖文件内容。

输入重定向
"<"将文件作为命令的标准输入
"<<"从标准输入中读取，知道遇见“分界符”才停止。
```bash
cat << EOF
EOF
```

## 第五节课
### 管道符、重定向与环境变量
#### 命令行通配符
##### 通配符
|通配符|含义|
|:|
|*|匹配零个或多个字符|
|?|匹配任意一个字符|
|[0-9]|匹配范围内的数字|
|[abc]|匹配abc三个字符中的任意一个单个字符|

##### 常用的转义字符
|字符|作用|
|:|
|\(反斜杠)|转义后面单个字符|
|''(单引号)|转义所有的字符|
|""(双引号)|变量依然生效|
|``(反引号)|执行命令语句|

```bash
echo "$(ip a)"
echo `ip a`
```

#### 环境变量
##### alias
##### unalias
##### type
```bash
#查看变量PATH的内容
echo $PATH
```

##### export 
##### unset 

##### 最重要的10个环境变量
|变量名称|作用|
|:|
|HOME|用户的主目录（家目录）|
|SHELL|用户在使用的SHELL解释器类型|
|HISTSIZE|历史命令记录条数|
|HISTFILESIZE|历史命令记录条数|
|MAIL|邮件信箱文件保存路径|
|LANG|系统语言、语系名称|
|RANDOM|生成一个随机数字|
|PS1|bash解释器的提示符|
|PATH|定义解释器搜索用户执行命令的路径|
|EDITOR|用户默认的文本编辑器|

全局变量
/etc/profile

局部变量
~/.bashrc
~/.bash_profile

### Vim编辑器与Shell命令脚本
#### Vim
Vim的三种模式
命令模式
编辑模式
末行模式

##### 命令模式
Esc键返回命令模式

|命令|作用|
|:|
|ZZ|保存并退出|
|ZQ|强制退出|

##### 编辑模式
aioAIO

##### 末行模式
Esc返回命令模式，按:键进入末行模式

|命令|作用|
|:|
|:wq|保存退出|
|:q!|强制退出|
|:set nu|显示行号|
|:set nonu|不显示行号|

```bash
#打开文件并显示行号
vim -c "set nu" 
```

## 第六节课
### Vim编辑器与Shell命令脚本
#### 配置网卡
/etc/sysconfig/network-scripts/ifcfg-eth0

#### 配置Yum仓库
之前的章节配置过了。
/etc/yum.repos.d/

```
vim /etc/yum.repos.d/rhel.repo
[rhel]
name=rhel
baseurl=http://mirrors.163.com
enabled=1
gpgcheck=0
```

### Shell脚本
/etc/shells

Shell的工作形式分为两种
交互式(Interactive)：用户输入一个
批处理(Batch)：

```bash
#!/usr/bin/env bash
#author:
#date:
#email:
```

#### 接收用户的参数
位置变量

```
./script.sh $1 $2 $3
```

$0 代表脚本本身的名字
$1 是脚本之后的第一个以空格分割的参数
$9
${10}
${11}
$#
$*
$?

#### 判断用户的参数
##### 测试语句格式
[ 条件表达式 ]

|操作符|作用|
|:|
|-d|测试是否为目录。|
|-e|测试文件或目录是否存在。|
|-f|判断是否为文件。|
|-r|测试当前用户是否有权限读取。|
|-w|测试当前用户是否有权限写入。|
|-x|	测试当前用户是否有权限执行|

##### 逻辑测试符号 
|操作符|作用|
|:|
|&&|逻辑的与，“而且”的意思。|
|&#124;&#124;|逻辑的非，“或者”的意思。|
|!|判断结果取相反值。|

```
[ $USER = root ] && echo "root"
```

##### 整数比较运算符
|操作符|作用|
|:|
|-eq|判断是否等于|
|-ne|判断是否不等于|
|-gt|判断是否大于|
|-lt|判断是否小于|
|-le|判断是否等于或小于|
|-ge|判断是否大于或等于|

```bash
free -m
free -m|grep Mem:
free -m|grep Mem:|awk '{print $4}'
FreeMem=`free -m|grep Mem:|awk '{print $4}'`
[ $FreeMem -lt 1024 ] && echo "Insufficient Memory"
```

##### 字符串比较
|操作符|作用|
|:|
|=|比较字符串内容是否相同。|
|!=|比较字符串内容是否不同。|
|-z|判断字符串内容是否为空。|

#### 流程控制语句
##### if条件测试语句
单分支结构

```bash
if then;
fi
```

双分支结构

```bash
if then;
else
fi
```
```bash
#/bin/bash
ping -c 3 -i 0.2 -W 3 $1 &> /dev/null
if [ $? -eq 0 ] then;
	echo "Host $1 is up"
else:
	echo "Host $1 is down"
fi
```

多重分支结构

```bash
if then;
elif then;
else
fi
```

read

```bash
#读取用户的输入
read -p
```
```bash
#!/bin/bash
read -p "Enter your socre(0-100)" GRADE
if [ $GRADE -ge 85 ]  && [$GRADE -le 100 ]  ; then
	echo "$GRADE is Excellent"
elif [ $GRADE -ge 70 ] && [$GRADE -le 84 ] ; then
	echo "$GRADE is pass"
else:
	echo "$GRADE is Fail"
fi
```

##### for条件循环语句
```bash
for in
do
done
```
```bash
#!/bin/bash
read -p "Enter the users password" PASSWD
for UNAME in `cat users.txt`
do
id $UNAME &> /dev/null
	if [ $? -eq 0 ] ;then
		echo "Aleady exists"
	else
		useradd $UNAME &> /dev/null
		echo "$PASSWD" | passwd --stdin $UNAME &> /dev/null
		if [ $? -eq 0 ] ;then
			echo "Create Sucess"
		else
			echo "Create Failure"
		fi
	fi
done
```

>Shell编程技巧 印度大神写的

##### while条件循环语句
```bash
while
do
done
```
```bash
#!/bin/bash
PRICE=$(expr $RANDOM % 1000)
TIMES=0
while true
do
read -p "PRICE:" INT
let TIMES++
if [ $INT -eq $PRICE ] ; then
	echo "ok"
	echo "$TIMES"
	exit 0
elif [ $INT -gt $PRICE ] ; then
	echo "high"
else
	echo "low"
done
```

##### case条件测试语句
```bash
case
)
;;
esac
```
```bash
#!/bin/bash
read -p "Enter:" KEY
case "$KEY" in
[a-z] | [A-Z])
echo "word"
;;
[0-9])
echo "number"
;;
*)
echo "error"
;;
esac
```

## 第七节课
### 计划任务服务
#### at
一次性任务atd服务/进程来实现的，计划的管理操作是“at”命令，

```bash
#安排一次性任务
echo "systemctl restart sshd " | at 23:00
#查看任务列表
at -l
#预览任务与环境设置
at -c
#删除任务
atrm
```

#### crontab
长期可循环的计划任务，cron服务

```bash
#查看任务列表
crontab -l 
#编辑任务
crontab -e
#删除任务
crontab -r
```

```
* 1 * * * /usr/bin/date >> /root/date.log
* 1,3,6 * * * /usr/bin/date >> /root/date.log
* 1-5 * * * /usr/bin/date >> /root/date.log
分钟 小时 日期 月份 星期 命令
```

|字段|说明|
|:|
|分钟|取值为从0到59之间的整数|
|小时|取值为从0到23之间的任意整数|
|日期|取值为1到31之间的任意整数|
|月份|取值为1到12之间的任意整数|
|星期|取值为0到7之间的任意整数，其中0与7均为星期日|
|命令|要执行的命令或程序脚本|

### 用户管理
UID(User IDentification)：每个用户都有对应的的UID值。
超级用户UID0:root用户默认为0。
系统用户UID1-999:系统中系统服务有不同用户运行，更加安全，默认被限制登录到系统。
普通用户UID1000~:管理员创建的普通用户UID从1000开始（即便前面的数字没有被使用完，也是从1000开始的。）
>账户名称与UID保存在/etc/passwd文件中，而账户密码则保存在/etc/shadow文件中。

GID(Group IDentification)：可将多个用户加入某个组中，方便指派任务或工作。
>用户组名称与GID保存在/etc/group文件中，组管理员的密码保存在/etc/gshaodow文件中。

#### useradd

|参数|作用|
|:|
|-d|指定用户的家目录（默认为/home/username）|
|-e|帐号有效截至日期，格式：YYYY-MM-DD.|
|-u|指定该用户的默认UID|
|-g|指定一个初始的用户基本组（必须已存在）|
|-G	|指定一个或多个扩展用户组|
|-N|不创建与用户同名的基本用户组|
|-s|指定该用户的默认Shell|

#### userdel
|参数|作用|
|:|
|-f|强制后删除用户，家目录与其相关的文件|
|-r|同时删除用户，家目录与其相关的文件|

#### usermod
|参数|作用|
|:|
|-c|填写帐号的备注信息|
|-d -m|-m与-d连用，可重新指定用户的家目录并自动把旧的数据转移过去。|
|-e|帐户到期时间，格式“YYYY-MM-DD”|
|-g|变更所属用户组|
|-G	|变更扩展用户组|
|-L|锁定用户禁止其登陆系统|
|-U	|解锁用户，允许其登陆系统|
|-s|变更默认终端|
|-u|修改用户的UID|

#### groupadd
#### passwd
|参数|作用|
|:|
|-l|锁定用户禁止其登陆|
|-u|解除锁定，允许用户登陆。|
|--stdin|允许从标准输入修改用户密码，如(echo "NewPassWord" &#124; passwd --stdin Username)|
|-d|使帐号无密码|
|-e|强制用户下次登陆时修改密码|
|-S|显示用户的密码状态|

### 文件权限与归属
>-:普通文件，d:目录文件，l:链接文件，b:块设备文件，c:字符设备文件，p:管道文件

-rwxrwxrwx

>读(read)，写(write)，执行（execute）简写即为(r,w,x)，亦可用数字(4,2,1)表示。

目录的w权限决定了这个目录下的文件是否可以删除。

#### 文件的特殊权限
>suid 4 sgid 2 sbit 1
##### SUID
SUID:让执行者临时拥有属主的权限（仅对拥有执行权限的二进制程序有效）

这是一种针对于二进制程序设置的特殊权限，例如所有用户都可以执行用于修改用户密码的passwd命令，而用户密码是被保存在/etc/shadow文件中的，但只要仔细的看下就会发现这个文件的默认权限是000，也就是说除了不受系统限制的root管理员用户以外的所有用户都没有查看或编辑该文件的权限。那么把passwd命令加上SUID特殊权限位，便可让普通用户临时获得程序所有者的身份，即以root用户的身份把变更的密码信息写入到shadow文件中。这个很像在古装剧中看过拿着尚方宝剑的钦差大臣，他手持尚方宝剑惩治贪官的时候实际上也就代表了皇上的权威，以便他完成了想要的效果，但这并不意味着他就永远成为了皇上，因此这是一种有条件的、临时的特殊权限授权方法。

##### SGID
SGID功能一：让执行者临时拥有属组的权限（对拥有执行权限的二进制程序设置）

这种特殊权限就是参考SUID而设计的，不同点就是让程序的执行者获取的不再是文件所有者的临时权限，而是获取到文件的所有组的权限。举例来说，在早期的Linux系统中/dev/kmem是一个字符设备文件，用于存储内核程序要访问的数据，

SGID功能二：在该目录中创建的文件自动继承此目录的用户组（只可以对目录设置）

正如前面提到过每个文件都有其归属的所有者和所有组，而当创建或传送一个文件后，这个文件就会自动的归属于执行这个操作的用户。那么比如工作中需要设置一个部门内的共享目录，让所有组内的人都能够读取里面的内容，那么咱们就可以创建部门共享目录后，把该目录设置上SGID特殊权限位，这样任何用户在里面创建的任何文件都会归属于本目录的所有组，而不再是自己的基本用户组。

##### SBIT
SBIT(Sticky Bit):只可管理自己的数据而不能删除他人文件(仅对目录有效)

很多大学老师要求学生下课后统一把作业上传到服务器的特定共享目录中，但老是有几个小破坏分子喜欢删除其他同学的作业，那就要设置SBIT特殊权限位了，当然也可以叫做特殊权限位之粘滞位。SBIT特殊权限位的设置目的和效果是不让其他人删除自己的文件，换句话说就是文件只能被所有者执行删除操作，不知道最初是那个呆瓜直接把英文Sticky Bit直译成了粘滞位，刘遄老师则推荐叫做保护位，既好记，又能让别人马上知道其功能作用。在RHEL7系统中的/tmp作为一个共享文件的目录默认已经被设置了SBIT特殊权限位，因此这里面的文件其他人是不能乱删除的。

##### chmod
##### chown

#### 文件的隐藏属性
##### chattr
|参数|作用|
|:|
|i|将无法对文件进行修改,若对目录设置后则仅能修改子文件而不能新建或删除。|
|a|仅允许补充（追加）内容.无法覆盖/删除(Append Only)。|
|S|文件内容变更后立即同步到硬盘(sync)。|
|s|彻底从硬盘中删除，不可恢复(用0填充原文件所在硬盘区域)。|
|A|不再修改这个文件的最后访问时间(atime)。|
|b|不再修改文件或目录的存取时间。|
|D|检查压缩文件中的错误。|
|d|当使用dump命令备份时忽略本文件/目录。|
|c|默认将文件或目录进行压缩。|
|u|当删除此文件后依然保留其在硬盘中的数据，方便日后恢复。|
|t|让文件系统支持尾部合并（tail-merging）。|
|X|可以直接访问压缩文件的内容。|

```bash
#将无法对文件进行修改,若对目录设置后则仅能修改子文件而不能新建或删除。
chattr +i 
```

##### lsattr

## 第八节课
### 文件权限与归属
#### 文件访问控制列表
##### setfacl
ACL提供的是在所有者、所有组、其他人的读写执行权限外的特殊权限控制，使用setfacl命令可以让咱们能够针对单一用户或用户组、单一文件或目录来进行读写执行权限的控制，其中对于目录文件需要使用递归-R参数，对普通文件需要使用-m参数，而如果想要删除某个文件的访问控制策略的话可以使用-b参数，于是快来设置下用户在/root目录上面的权限吧。

是不是觉得效果很酷呢？但是又遇到了一个小问题——怎么样去查看文件上面有那些ACL策略呢？常用的ls命令是看不到访问控制列表信息的，但是却可以看到文件的权限最后一个点(.)变成了加号（+）,而这就意味着这个文件已经被设置有了ACL访问策略啦。现在是不是感觉学得越多，越不敢说自己精通于Linux系统了呢？这么一个不起眼的点(.)竟然还意味着这么一种重要的权限呢。

```bash
setfacl -Rm u:linuxprobe:rwx /root
```

>ACL大于特殊权限(SUID、SGID、SBIT)

##### getfacl

#### su命令与sudo服务
##### su
su命令与用户名之间有一个减号(-)，这意味着完全的切换到新的用户，即把环境变量信息也变更为新的用户，而不保留原始的用户信息，这个是推荐必加的参数，一定要记下哦~另外当超级用户切换到普通用户时是不需要密码验证的，而普通用户切换成超级用户身份就需要密码验证后才能成功了。

##### sudo
1. 限制用户执行指定的命令。
2. 记录用户执行的每一条命令。
3. 配置文件（/etc/sudoers）提供集中的管理用户、权限与主机等参数。
4. 验证过密码后5分钟(默认值)内无须再让用户验证密码，更加的方便。

|参数|作用|
|:|
|-h|列出帮助信息。|
|-l|列出当前用户可执行的命令。|
|-u|用户名或UID值	以指定的用户身份执行命令。|
|-k|清空安全时间，下次执行sudo时需要再次密码验证。|
|-b|在后台执行指定的命令。|
|-p|更改询问密码的提示语。|

##### visudo
```
username ALL=(ALL)  NOPASSWD: ALL
用户名 所有 不用输入密码
```

### 存储结构与磁盘划分
#### Linux系统中的文件存储结构
FHS(Filesystem Hierarchy Standard)

刚刚提到的FHS文件系统层次结构标准协定（Filesystem Hierarchy Standard）是工程师们在Linux系统中存储文件的时候需要遵守的规则，FHS是根据以往无数Linux系统用户和开发者的经验而总结出来的，指导咱们应该把文件保存到什么位置，以及告诉运维人员应该在何处找到所需的文件。但目前FHS对于运维工程师来讲也算是一种道德操守，有些运维人员就是懒得遵守，或者根本没有听说过这个东东，依然会把文件到处乱放。刘遄老师并不要求同学们去谴责他们，而应该把自己的知识活学活用，千万不要认准这个FHS协定只讲死道理，那吃亏的就可能是自己啦，最常见的目录所对应的用处包括有：

|目录名称|应放置文件的内容|
|:|
|/boot|开机所需文件——内核,开机菜单及所需配置文件等|
|/dev|任何设备与接口都以文件形式存放在此目录|
|/etc|配置文件|
|/home|用户主目录|
|/bin|单用户维护模式下还能够被操作的命令|
|/lib|开机时用到的函数库及/bin与/sbin下面命令要调用的函数|
|/sbin|开机过程中需要的|
|/media|一般挂载或删除的设备|
|/opt|放置第三方的软件|
|/root|系统管理员的主文件夹|
|/srv|一些网络服务的数据目录|
|/tmp|任何人均可使用的“共享”临时目录|
|/proc|虚拟文件系统，例如系统内核，进程，外部设备及网络状态等|
|/usr/local|用户自行安装的软件|
|/usr/sbin|非系统开机时需要的软件/命令/脚本|
|/usr/share|帮助与说明文件，也可放置共享文件。|
|/var|主要存放经常变化的文件，如日志。|
|/lost+found|当文件系统发生错误时，将一些丢失的文件片段存放在这里|

#### 路径
路径指的是如何定位到某个文件，分为绝对路径与相对路径。在Linux系统中的绝对路径指的是从根目录（/）开始写起的文件或目录名称，而相对路径则指的是相对于当前路径的写法。

绝对路径(absolute): /root/dir/a/b/c
相对路径(relative): ./dir/a/b/c

#### 物理设备的命名规则
Linux系统中一切都是文件，那么硬件设备肯定也不例外。既然是文件就必须有名称啦，系统内核的udev设备管理器会自动把硬件名称规范起来，目的是让运维人员可以通过设备文件的名字猜出设备大致的属性以及分区信息等，对于陌生的设备这点特别的方便，另外udev服务会一直以守护进程的形式运行并侦听来自内核发出的信号来管理/dev目录下的设备文件。常见的硬件命名规则如下：

|硬件设备	文件名称|
|:|
|IDE设备|/dev/hd[a-d]|
|SCSI/SATA/U盘	|/dev/sd[a-p]|
|软驱|/dev/fd[0-1]|
|打印机|/dev/lp[0-15]|
|光驱|/dev/cdrom|
|鼠标|/dev/mouse|
|磁带机|/dev/st0或/dev/ht0(IDE设备)|

因为现在的IDE设备已经很少见啦，所以一般硬盘设备都会是以“/dev/sd”开头的，而一台主机上可以有多块硬盘，因此系统便会用a-p来代表16块不同的硬盘（默认从a开始分配）且分区编号也很有讲究。

>主分区或扩展分区的编号从1开始至4结束。
>逻辑分区从编号5开始。

#### MBR
计算机中有了硬盘设备才使得游戏通关过后可以保存记录而不是再每次再重头开始，硬盘设备则是由大量的扇区组成的，其中第一个扇区最重要，它里面保存着主引导记录与分区表信息。单个扇区容量为512bytes组成，主引导记录需要占用446bytes，分区表的为64bytes，结束符占用2bytes，而其中每记录一个分区信息需要16bytes，这最多四个能有幸被写到第一个扇区中的分区信息就叫做主分区。

#### 文件系统与数据资料
用户在硬件存储设备上面正常建立文件、写入，读取，修改，转存文件与控制文件等等操作都是依靠了文件系统来完成的，文件系统的作用是把硬盘合理的规划，保证用户正常的使用需求，在Linux系统中支持的文件系统就有数十种，而最常见的文件系统有：

Ext3是一款日志文件系统，能够在异常宕机中避免文件系统资料丢失的情况，自动修复数据的不一致与错误，然而对于容量较大的硬盘来说需要耗费在修复上的时间也会非常多，并且也不能保证100%资料不流失。它会把整个磁盘的每个写入动作细节都预先记录下来，以便在异常宕机后能够回溯追踪到被中断的部分。

Ext4可以称为是Ext3的后继版本，作为RHEL6系统中的默认文件管理系统，它支持更大的文件系统到1EB（1EB=1,073,741,824GB且能够有无限多的子目录），另外Ext4文件系统能够批量分配block块并作"Extents"极大的提高了读写效率。

XFS作为最新RHEL7中默认的文件管理系统，它的日志型文件管理系统的优势在意外宕机后尤其明显，可以快速的恢复可能被破坏的文件，另外经过优化后日志功能对硬盘性能影响非常小，同时最大支持18EB的存储容量满足了几乎所有需求。

##### inode
日常中在硬盘要保存的数据实在太多了，因此就要有个叫super block的“硬盘地图”，并不是把数据直接写入到这个“大地图”里面，而是在上面记录着整个文件系统的信息，因为如果把所有的信息都写入到这里面的话，就一定会导致它的体积变的很大，查询与写入速度会变的很慢，于是每个文件的权限与属性都会记录在inode中（每个文件都会占用一个独立的inode表格，默认为128bytes），记录着：

>该文件的访问权限(read,write,execute)
>该文件的所属主与组(owner,group)
>该文件的大小(size)
>该文件的创建或状态修改时间(ctime)
>该文件的最后一次访问时间(atime)
>该文件的修改时间(mtime)
>文件的特殊权限(SUID,SGID,SBIT)
>该文件的真实数据地址(point)

##### block
而文件的实际数据内容则保存在block块中（大小可以是1K、2K或4K），一个inode大小仅为128bytes（Ext3），记录一个block消耗4bytes，一般当把inode写满后就会取出一个block用于号码记录而不再是保存实际的文件系统。下面的说明中以4K为例。

>情况一：文件体积很小（1K），那么依然会占用一个block，潜在的浪费3K。
>情况二：文件体积很大（5K），那么会占用两个（5K-4K剩下的1K也要占用一个block）。

##### VFS
实际上随着计算机系统的发展产生出了众多的文件系统，为了使用户在读取或写入文件时不用关心底层的硬盘结构，于是在Linux内核中的软件层为用户程序提供了一个VFS文件系统接口(Virtual File System)，这样就转而统一对这个虚拟文件系统进行操作啦。实际文件系统在VFS下隐藏了自己的特性和细节，使得咱们在日常使用时觉得“文件系统都是一样的”，比如使用cp命令就在任何文件系统中复制文件了。

## 第九节课
### 存储结构与磁盘划分
#### 挂载硬件设备
##### mount
|参数|作用|
|-a|挂载所有在/etc/fstab中定义的文件系统|
|-t|指定文件系统的类型|

***
/etc/fstab

/dev/sdb2 /backup ext4 defaults 0 0

填写格式如下：“设备文件 挂载目录 格式类型 权限选项 自检 优先级”

设备文件：一般为设备的路径+设备名称，也可以写UUID值。
挂载目录：指定要挂载到的目录，需挂载前创建好。
格式类型：即指定文件系统的格式，比如有ext3/ext4/xfs/swap/iso9660（此为光盘设备）等等。
权限选项：默认为defaults(rw,suid,dev,exec,auto,nouser,async)，可指定acl或quota等。
自检：若为1则开机后进行磁盘自检，0为不自检。
优先级：若“自检”为1，则可对多块硬盘进行优先级设置。
***

##### umount
```
umount /dev/sdb2
```

##### fdisk
|参数|作用|
|:|
|m|查看全部可用的参数|
|n|添加新的分区|
|d|删除某个分区信息|
|l|列出所有可用的分区类型|
|t|改变某个分区的类型|
|p|查看分区表信息|
|w|保存并退出|
|q|不保存直接退出|

```bash
#进入fdisk交互式分区界面
fdisk /dev/sdb
#查看分区
fdisk /dev/sdb -l
```

##### mkfs
```
mkfs.ext4 /dev/sdb2
```

##### df
```
df -h
```

##### du
```bash
#查看占用多大硬盘空间
du -sh
```

#### 添加交换分区
```bash
#格式化swap分区
mkswap /dev/sdb5
#挂载swap分区
swapon /dev/sdb5

#开启自动挂载
vi /etc/fstab
/dev/sdb5 swap swap defaults 0 0
```

#### 磁盘容量配额
Linux系统的设计初衷理念就让许多人一起使用，成为多用户、多任务的操作系统，但是硬件的资源是固定有限的，如果出现个小破坏份子不断的创建文件或下载电影，那么硬盘空间总有一天会被占满的吧.这时就需要磁盘配额服务帮助管理员限制某用户或某个用户组对特定文件夹可以使用的最大硬盘空间，一旦超出预算就不再允许他们继续使用。quota服务做磁盘配额可以限制用户的硬盘可用量或最大创建文件数量，并且还有软、硬限制的功能：

>软限制:当达到软限制时会提示用户，但允许用户在规定额度内继续使用。
>硬限制:当达到硬限制时会提示用户，且强制终止用户的操作。

对于学习过早期Linux系统或有红帽RHEL6系统使用经验的同学可能会犯一个小错误，因为早期Linux系统想让设备支持quota磁盘配额服务的是usrquota参数，而在本系统中则是uquota参数，然后重启系统后就能使用mount命令看到/boot目录已经支持了quota磁盘配额技术啦。

```
vi /etc/fstab
/dev/sdb4 /mnt/xfs xfs defaults,uquota 1 1
```

##### xfs_quota
其中-c参数用于以参数的形式设置要执行的命令，-x参数是专家模式，让运维人员能够对quota服务做更多复杂的配置。那么就来用xfs_quota命令设置tom用户对/boot目录的磁盘配额吧，具体的限额控制包括有硬盘使用软限制为3M，硬盘使用硬限制为6M，创建文件数量软限制为3个，创建文件硬限制为6个。

```bash
xfs_quota -x -c 'limit bsoft=3m bhard=6m isoft=3 ihard=6 tom' /boot

xfs_quota -x -c report /boot
```

##### edquota
在为用户设置了quota磁盘配额限制后可以用edquota来随着需求的变化而进一步修改限额的数值，其中-u参数代表要针对那个用户进行的设置，-g参数则代表要针对那个用户组进行的设置，edquota命令会调用vi或vim编辑器来让用户修改要限制的项目。

#### 软硬方式链接
硬链接(hard link)可以被理解为一个“指向原始文件inode的指针”，系统不为它分配独立的inode与文件，所以实际上来讲硬链接文件与原始文件其实是同一个文件，只是名字不同。于是每添加一个硬链接，该文件的inode连接数就会增加1，直到该文件的inode连接数归0才是彻底删除。也就是说因为硬链接实际就是指向原文件inode的指针，即便原始文件被删除依然可以通过链接文件访问，但是由于技术的局限性而不能跨文件系统也不能链接目录文件。

软链接也称为符号链接（symbolic link）即“仅仅包含它所要链接文件的路径名”因此能做目录链接也可以跨越文件系统，但原始文件被删除后链接文件也将失效，性质上和Windows™系统中的“快捷方式”是一样的。

##### ln
|参数|作用|
|:|
|-s|创建"符号链接"(默认是硬链接)|
|-f|强制创建文件或目录的链接|
|-i|覆盖前先询问|
|-v|显示创建链接的过程|

### [RAID](http://www.linuxprobe.com/chapter-07.html#71_RAID)
RAID(Redundant Array of Independent Disks)磁盘冗余阵列

RAID0 备份
RAID1 速度快
RAID5 容量大
RAID10 RAID1+RAID1 备份+速度快

#### 部署磁盘阵列组
##### mdadm
在现在生产环境中的服务器一般都会配备有RAID阵列卡，价格也是越来越廉价，但没有必要让同学们为了做一个实验而单独去买一台服务器，mdadm命令能够在Linux系统中创建和管理软件RAID磁盘阵列组，对于其中的理论知识和操作过程是与生产环境保持一致的~mdadm命令的常用参数包括。

mdadm管理RAID阵列的动作

|名称|作用|
|:|
|Assemble|将设备加入到以前定义的阵列|
|Build|创建一个没有超级块的阵列|
|Create|创建一个新的阵列，每个设备具有超级块|
|Manage|管理阵列（如添加和删除）|
|Misc|允许单独对阵列中的某个设备进行操作（如停止阵列）|
|Follow or Monitor|监控状态|
|Grow|改变阵列的容量或设备数目|

mdadm管理RAID阵列的参数

|参数|作用|
|:|
|-a|检测设备名称|
|-n|指定设备数量|
|-l|指定raid级别|
|-C|创建|
|-v|显示过程|
|-f|模拟设备损坏|
|-r|移除设备|
|-Q|查看摘要信息|
|-D	|查看详细信息|
|-S|停止阵列|

使用mdadm命令创建RAID10,名称为"/dev/md0"。

第6章中讲到过udev是Linux系统内核中用来给硬件命名的服务，命名规则也非常的简单，可以猜测到第二个SCSI存储设备的名称会是为/dev/sdb，然后以此类推。使用硬盘设备做RAID磁盘阵列组很像几个同学组成一个班级，但班级的名称总不能叫做/dev/sdbsdcsddsde吧，这样虽然可以一眼看出是由那些元素组成的，但明显非常的不利于记忆和阅读吧，更何况如果是用10、50、100个硬盘组成的设备呢？因此需要用-C参数代表创建一个RAID阵列卡，-v参数来显示出创建的过程，同时就在后面追加一个设备名称，这样以后/dev/md0就是创建出RAID磁盘阵列组的名称啦，-a yes参数代表自动创建设备文件，-n 4参数代表使用4块硬盘来制作这个RAID磁盘阵列组，而-l 10参数则代表RAID10方案，最后面再加上4块硬盘设备的名称就搞定啦：

```bash
mdadm -Cv /dev/md0 -a yes -n 4 -l 10 /dev/sdb /dev/sdc /dev/sdd /dev/sde

mkfs.ext4 /dev/md0

mkdir /RAID
mount /dev/md0 /RAID

#查看/dev/md0磁盘阵列设备组详细信息
mdadm -D /dev/md0
#把挂载信息写入到配置文件中永久生效
echo "/dev/md0 /RAID ext4 defaults 0 0" >> /etc/fstab
```

#### 损坏磁盘阵列及修复
咱们在生产环境中部署RAID10磁盘阵列组目的就是为了提高存储设备的IO读写速度及数据的安全性，但因为这次是在本机电脑上模拟出来的硬盘设备所以对于读写速度的改善可能并不直观，因此刘遄老师决定给同学们讲解下RAID磁盘阵列组损坏后的处理方法，这样以后步入了运维岗位后不会因为突发事件而手忙脚乱。首先确认有一块物理硬盘设备出现损坏不能再继续正常使用后，应该使用mdadm命令来予以移除之后查看下RAID磁盘阵列组的状态已经被改变：

```bash
mdadm /dev/md0 -f /dev/sdb
mdadm -D /dev/md0
mdadm /dev/md0 -a /dev/sdb
mdadm -D /dev/md0
mount -a
```

####  磁盘阵列组+备份盘
同学们有没有想到还有一种极端情况，RAID10最多允许损坏50%的硬盘设备，但如果同一组中的设备同时全部损坏也会导致数据丢失，换句话说，如果当RAID10磁盘阵列组中某一块硬盘出现了故障，而咱们正在前往修复它的路上，恰巧此时同RAID1组中的另一块硬盘设备也出现故障，那么数据就被彻底损坏了。哈哈，刘遄老师可不是乌鸦嘴，这种同时损坏的情况还真的在我的学生中出现过，那这样情况该怎么办呢？其实就可以使用RAID备份盘技术来预防这类事故，顾名思义就是准备一块足够大的硬盘，这块硬盘设备平时是闲置状态不用工作，一旦RAID磁盘阵列组中有硬盘出现故障后则会马上自动顶替上去，这样很棒吧！

咱们现在来创建一个RAID5磁盘阵列组+备份盘吧，-n 3参数代表创建这个RAID5所需的硬盘个数，-l 5参数代表RAID磁盘阵列的级别，而-x 1参数则代表有1块备份盘，当查看/dev/md0磁盘阵列组的时候就能看到有一块备份盘在等待中了。

```bash
mdadm -Cv /dev/md0 -n 3 -l 5 -x 1 /dev/sdb /dev/sdc /dev/sdd /dev/sde
mdadm -D /dev/md0
mkfs.ext4 /dev/md0
echo "/dev/md0 /RAID ext4 defaults 0 0" >> /etc/fstab
mkdir /RAID
mount -a
```

最后就是见证奇迹的时刻啦，咱们再次把硬盘设备/dev/sdb移出磁盘阵列组，这样快速看下/dev/md0磁盘阵列组的状态就会发现备份盘已经被自动顶替上去，这是非常实用的，在RAID磁盘阵列组数据安全保证的基础上进一步提高数据可靠性，所以如果您的公司不差钱的话还是再买上一块备份盘以防万一吧。

```bash
mdadm /dev/md0 -f /dev/sdb
mdadm -D /dev/md0
```

## 第十节课
### LVM逻辑卷管理器
LVM(Logical Volume Manager)

LVM逻辑卷管理器是一项理论性较强的技术，它是Linux系统中对硬盘分区进行管理的一种机制，开创这项技术的初衷是为了解决传统硬盘分区创建后不易更改其大小的弱点，对于传统硬盘分区进行强制扩容和缩小技术理论上虽然是可行的，但却有可能造成数据的丢失，LVM逻辑卷管理器技术能够把多块硬盘进行卷组合并，让用户不必关心设备底层的架构和布局，拿来即用。LVM逻辑卷管理器是在磁盘分区和文件系统之间添加的逻辑层，它提供了一个抽象的卷组，可以使得多块硬盘进行卷组合并，让用户不必关心物理硬盘设备的底层结构，从而实现对分区的灵活动态调整。

LVM逻辑卷管理器的技术结构（物理卷PV，Physical Volume）合并成（卷组VG，Volume Group），然后把这卷组再分割成一个个（逻辑卷LV，Logical Volume），每个逻辑卷的大小必须是（基本单元PE，Physical Extent）的倍数。物理卷是处于逻辑卷管理器中最底层的资源，可以理解成是物理硬盘、硬盘分区或者RAID磁盘阵列组都可以，而卷组是建立在物理卷之上的，一个卷组中可以包含多个物理卷，当然在卷组创建之后也可以继续向其中添加新的物理卷，而逻辑卷是建立于卷组之上的，把卷组中空闲的资源建立出新的逻辑卷，并且逻辑卷建立后可以动态的扩展或缩小空间，这也就是LVM逻辑卷管理器的核心理念。

底层PV合并成VG，VG分割LV，新建LV时要指定PE（-l 每个PE默认4MB）

#### 部署逻辑卷
很多时候最初咱们都不能够精准的评估和分配每个硬盘分区以后的使用情况，有可能会随着业务量的增加导致数据库目录的体积也增加，也有可能需要分析用户行为而做记录导致日志目录的体积不断变大，或者有些较小概率还要对原先错误分配过大的分区进行精简，那么这些知识都属于这个章节的学习范畴。并且如刚刚提到过的，LVM逻辑卷管理器是对Linux系统中对存储资源进行管理的一种机制，部署LVM逻辑卷管理器需要依次对对物理卷、卷组和逻辑卷的逐个配置，常见的命令分别包括有：

|功能/命令|物理卷管理|卷组管理|逻辑卷管理|
|:|
|扫描|pvscan|vgscan|lvscan|
|建立|pvcreate|vgcreate|lvcreate|
|显示|pvdisplay|vgdisplay|lvdisplay|
|删除|pvremove|vgremove|lvremove|
|扩展||vgextend|lvextend|
|缩小||vgreduce|lvreduce|

```bash
#让新添加的两块硬盘设备支持LVM逻辑卷管理器技术
pvcreate /dev/sdb /dev/sdc
#把两块硬盘设备都加入到storage卷组中，然后查看下卷组的状态
vgcreate storage /dev/sdb /dev/sdc
#切割出一个约为150M的逻辑卷设备
# 需要注意下切割单位的问题，在LVM逻辑卷管理器对LV逻辑卷的切割上面有两种计量单位，第一种是常见以-L参数来以容量单位为对象，例如使用-L 150M来生成一个大小为150M的逻辑卷，还可以使用-l参数来指定要使用PE基本单元的个数，默认每个PE的大小为4M，因此允许使用-l 37来生成一个大小为37*4M=148M的逻辑卷
lvcreate -n vo -l 37 storage
#查看逻辑卷
lvdisplay
#把生成好的逻辑卷格式化后挂载使用
# Linux系统会把LVM逻辑卷管理器中的逻辑卷设备存放在/dev设备目录中（实际上是做了一个符号链接，但读者们无需关心），同时会以卷组的名称来建立一个目录，其中保存有逻辑卷的设备映射文件（即/dev/卷组名称/逻辑卷名称）。
mkfs.ext4 /dev/storage/vo
mkdir /mnt/vo
mount /dev/storage/vo /mnt/vo
#查看挂载状态，并写入到配置文件永久生效
df -h
echo "/dev/storage/vo /mnt/vo ext4 defaults 0 0" >> /etc/fstab
```

####  扩容逻辑卷
虽然卷组是由两块硬盘设备共同组成的，但用户使用存储资源时感知不到底层硬盘的结构，也不用关心底层是由多少块硬盘组成的，只要卷组中的资源足够就可以一直为逻辑卷扩容，扩展前请一定要记得卸载设备和挂载点的关联。

```bash
#卸载 /mnt/vo
umount /mnt/vo
#把上个实验中的逻辑卷vo扩展至290M
lvextend -L 290M /dev/storage/vo
#检查磁盘完整性，重置硬盘容量
e2fsck -f /dev/storage/vo
#重新挂载硬盘设备并查看挂载状态
mount -a
df -h
```

####  缩小逻辑卷
相比于扩容逻辑卷来讲，对逻辑卷的缩小操作存在着更高丢失数据的风险，所以在生产环境中同学们一定要留心记得提前备份好数据，另外Linux系统规定对LVM逻辑卷的缩小操作需要先检查文件系统的完整性，当然这也是在保证咱们的数据安全，操作前记得先把文件系统卸载掉。

```bash
umount /mnt/vo
#检查文件系统的完整性
e2fsck -f /dev/storage/vo
#把LV逻辑卷的容量减小到120M
resize2fs /dev/storage/vo 120M
#把文件系统重新挂载并查看系统状态
mount -a
df -h
```

#### 逻辑卷快照
除此之外LVM逻辑卷管理器还具备有“快照卷”的功能，这项功能很类似于虚拟机软件的还原时间点功能。例如可以对某一个LV逻辑卷设备做一次快照，如果今后发现数据被改错了，咱们可以把之前做好的快照卷进行覆盖还原，LVM逻辑卷管理器的快照功能有两项特点，第一是快照卷的大小应该尽量等同于LV逻辑卷的容量，第二是快照功能仅一次有效，一旦被还原后则会被自动立即删除，首先应当查看下卷组的信息。

```bash
vgdisplay
#使用-s参数来生成一个快照卷，使用-L参数来指定切割的大小，另外要记得在后面写上这个快照是针对那个LV逻辑卷设备做的。
lvcreate -L 120M -s -n SNAP /dev/storage/vo
#在LV设备卷所挂载的目录中创建一个100M的垃圾文件，这样再来看快照卷的状态就会发现使用率上升了。
dd if=/dev/zero of=/linuxprobe/files count=1 bs=100M
lvdisplay
#为了校验SNAP快照卷的效果，需要对逻辑卷进行快照合并还原操作，在这之前记得先卸载掉逻辑卷设备与目录的挂载
umount /mnt/vo
lvconvert --merge /dev/storage/SNAP
#快照卷会被自动删除掉，并且刚刚在逻辑卷设备被快照后再创建出来的100M垃圾文件也被清除了。
mount -a
ls /mnt/vo
```

#### 删除逻辑卷
当生产环境中想要重新部署或者不需要再继续使用LVM逻辑卷管理器了，除了提前备份好重要数据信息，还必须依次删除LV逻辑卷、VG卷组后再移除PV物理卷设备，这样的顺序不可颠倒。

```bash
#取消逻辑卷与目录的挂载关联，删除配置文件中永久生效的设备参数。
umount /mnt/vo
删除/etc/fstab内的开机挂载信息
#把LV逻辑卷设备删除，需要手工输入y来确认操作
lvremove /dev/storage/vo
#把VG卷组删除，此处只需写卷组名称即可，而无需设备完整绝对路径。
vgremove storage
#把PV物理卷设备移除
pvremove /dev/sdb /dev/sdc
```

### 防火墙
#### 了解防火墙管理工具
保证数据的安全性是继可用性之后最为重要的一项工作，众所周知外部公网相比企业内网更加的“罪恶丛生”，因此防火墙技术作为公网与内网之间的保护屏障，虽然有软件或硬件之分，但主要功能都是依据策略对外部请求进行过滤。防火墙技术能够做到监控每一个数据包并判断是否有相应的匹配策略规则，直到匹配到其中一条策略规则或执行默认策略为止，防火墙策略可以基于来源地址、请求动作或协议等信息来定制，最终仅让合法的用户请求流入到内网中，其余的均被丢弃。

#### iptables
##### 规则链与策略
防火墙会从上至下来读取规则策略，一旦匹配到了合适的就会去执行并立即结束匹配工作，但也有转了一圈之后发现没有匹配到合适规则的时候，那么就会去执行默认的策略。因此对防火墙策略的设置无非有两种，一种是“通”，一种是“堵”——当防火墙的默认策略是拒绝的，就要设置允许规则，否则谁都进不来了，而如果防火墙的默认策略是允许的，就要设置拒绝规则，否则谁都能进来了，起不到防范的作用。

iptables命令把对数据进行过滤或处理数据包的策略叫做规则，把多条规则又存放到一个规则链中，规则链是依据处理数据包位置的不同而进行的分类，包括有：在进行路由选择前处理数据包（PREROUTING）、处理流入的数据包（INPUT）、处理流出的数据包（OUTPUT）、处理转发的数据包（FORWARD）、在进行路由选择后处理数据包（POSTROUTING）。从内网向外网发送的数据一般都是可控且良性的，因此显而易见咱们使用最多的就是INPUT数据链，这个链中定义的规则起到了保证私网设施不受外网骇客侵犯的作用。

比如您所居住的社区物业保安有两条规定——“禁止小商贩进入社区，各种车辆都需要登记”，这两条安保规定很明显应该是作用到了社区的正门（流量必须经过的地方），而不是每家每户的防盗门上。根据前面提到的防火墙策略的匹配顺序规则，咱们可以猜想有多种情况——比如来访人员是小商贩，则会被物业保安直接拒绝在大门外，也无需再对车辆进行登记，而如果来访人员是一辆汽车，那么因为第一条禁止小商贩策略就没有被匹配到，因而按顺序匹配到第二条策略，需要对车辆进行登记，再有如果来访的是社区居民，则既不满足小商贩策略，也不满足车辆登记策略，因此会执行默认的放行策略。

不过只有规则策略还不能保证社区的安全，物业保安还应该知道该怎么样处理这些被匹配到的流量，比如包括有“允许”、“登记”、“拒绝”、“不理他”，这些动作对应到iptables命令术语中是ACCEPT（允许流量通过）、LOG（记录日志信息）、REJECT（拒绝流量通过）、DROP（拒绝流量通过）。允许动作和记录日志工作都比较好理解，着重需要讲解的是这两条拒绝动作的不同点，其中REJECT和DROP的动作操作都是把数据包拒绝，DROP是直接把数据包抛弃不响应，而REJECT会拒绝后再回复一条“您的信息我已收到，但被扔掉了”，让对方清晰的看到数据被拒绝的响应。就好比说您有一天正在家里看电视，突然有人敲门，透过“猫眼”一看是推销商品的，咱们如果不需要的情况下就会直接拒绝他们（REJECT）。但如果透过“猫眼”看到的是债主带了几十个小弟来讨债，这种情况不光要拒绝开门，还要默不作声，伪装成自己不在家的样子（DROP），这就是两种拒绝动作的不同之处。

ACCEPT（允许流量通过）
LOG（记录日志信息）
REJECT（拒绝流量通过）
DROP（拒绝流量通过）

##### 规则链
规则链依据处理数据包的位置不同而进行分类：

|位置|含义|
|:|
|PREROUTING|在进行路由选择前处理数据包|
|INPUT|处理入站的数据包|
|OUTPUT|处理出站的数据包|
|FORWARD|处理转发的数据包|
|POSTROUTING|在进行路由选择后处理数据包|

##### 规则表
iptables中的规则表是用于容纳规则链，规则表默认是允许状态的，那么规则链就是设置被禁止的规则，而反之如果规则表是禁止状态的，那么规则链就是设置被允许的规则。

|表名|含义|
|:|
|raw表|确定是否对该数据包进行状态跟踪|
|mangle表|为数据包设置标记|
|nat表|修改数据包中的源、目标IP地址或端口|
|filter表|确定是否方形该数据包（过滤）|

规则表的先后顺序：raw->mangle->nat->filter

规则链的先后顺序：
>入站顺序：PREROUTE->INPUT
>出站顺序：OUTPUT->POSTROUTING
>转发顺序：PREROUTING->FORWARD->POSTROUTING

三点注意事项：
1. 没有指定规则表则默认指filter表
2. 不指定规则链则指表内所有的规则链
3. 在规则链中匹配规则时会依次检查，匹配即停止（LOG规则例外），若没有匹配项则按链的默认状态处理。

##### SNAT和DNAT
SNAT
SNAT源地址转换技术，能够让多个内网用户通过一个外网地址上网，解决了IP资源匮乏的问题。

```bash
#在使用ADSL动态IP上网，外网IP地址不稳定的情况可使用MASQUERADE(动态伪装)，能够自动的寻找外网地址并改为当前正确的外网IP地址。
iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -j MASQUERADE
```

DNAT
DNAT目的地地址转换技术，则能够让外网IP用户访问局域网内不同的服务器。

```bash
#互联网访问内网192.168.1.5这台提供web服务的服务器，在网关系统上运行此命令。
iptables -t nat -A PREROUTING -i eth0 外网IP -p tcp --dport 80 -j DNAT --to-destination 192.168.1.5
```


##### 基本的命令参数
iptables是一款基于命令行的防火墙策略管理工具，由于该命令是基于终端执行且存在有大量参数的，学习起来难度还是较大的，好在对于日常控制防火墙策略来讲，您无需深入的了解诸如“四表五链”的理论概念，只需要掌握常用的参数并做到灵活搭配即可，以便于能够更顺畅的胜任工作所需。iptables命令可以根据数据流量的源地址、目的地址、传输协议、服务类型等等信息项进行匹配，一旦数据包与策略匹配上后，iptables就会根据策略所预设的动作来处理这些数据包流量，另外再来提醒下同学们防火墙策略的匹配顺序规则是从上至下的，因此切记要把较为严格、优先级较高的策略放到靠前位置，否则有可能产生错误。

|参数|作用|
|:|
|-P|设置默认策略:iptables -P INPUT (DROP&#124;ACCEPT)|
|-F|清空规则链|
|-L|查看规则链|
|-A|在规则链的末尾加入新规则|
|-I num|在规则链的头部加入新规则|
|-D num|删除某一条规则|
|-s|匹配来源地址IP/MASK，加叹号"!"表示除这个IP外。|
|-d|匹配目标地址|
|-i 网卡名称|匹配从这块网卡流入的数据|
|-o 网卡名称|匹配从这块网卡流出的数据|
|-p|匹配协议,如tcp,udp,icmp|
|--dport|num	匹配目标端口号|
|--sport|num	匹配来源端口号|

```bash
#查看已有的防火墙策略
iptables -L
#清空已有的防火墙策略
iptables -F
#把INPUT链的默认策略设置为拒绝：
# 如前面所提到的防火墙策略设置无非有两种方式，一种是“通”，一种是“堵”，当把INPUT链设置为默认拒绝后，就要往里面写入允许策略了，否则所有流入的数据包都会被默认拒绝掉，同学们需要留意规则链的默认策略拒绝动作只能是DROP，而不能是REJECT。
iptables -P INPUT DROP
#向INPUT链中添加允许icmp数据包流入的允许策略：
# 在日常运维工作中经常会使用到ping命令来检查对方主机是否在线，而向防火墙INPUT链中添加一条允许icmp协议数据包流入的策略就是默认允许了这种ping命令检测行为。
iptables -I INPUT -p icmp -j ACCEPT
#删除INPUT链中的那条策略，并把默认策略还原为允许
iptables -D INPUT 1
iptables -D INPUT ACCEPT
#设置INPUT链只允许指定网段访问本机的22端口，拒绝其他所有主机的数据请求流量：
# 防火墙策略是按照从上至下顺序匹配的，因此请一定要记得把允许动作放到拒绝动作上面，否则所有的流量就先被拒绝掉了，任何人都获取不到咱们的业务。
iptables -I INPUT -s 192.168.1.0/24 -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -p tcp --dport 22 -j REJECT
#向INPUT链中添加拒绝所有人访问本机12345端口的防火墙策略
iptables -I INPUT -p tcp --dport 12345 -j REJECT
iptables -I INPUT -p udp --dport 12345 -j REJECT
#向INPUT链中添加拒绝来自于指定192.168.10.5主机访问本机80端口（web服务）的防火墙策略
iptables -I INPUT -p tcp -s 192.168.10.5 --dport 80 -j REJECT
#向INPUT链中添加拒绝所有主机不能访问本机1000至1024端口的防火墙策略
iptables -A INPUT -p tcp --dport 1000:1024 -j REJECT
iptables -A INPUT -p udp --dport 1000:1024 -j REJECT
#禁止用户访问www.baidu.com
iptables -I FORWARD -d www.baidu.com -j DROP
#禁止IP地址是192.168.1.5的用户上网
iptables -I FORWARD -s 192.168.1.5 -j DROP
#想让配置的防火墙策略永久的生效下去，还要执行一下保存命令
service iptables save
```

## 第十一节课
### 防火墙
#### Firewalld防火墙
RHEL7是一个集合多款防火墙管理工具并存的系统，Firewalld动态防火墙管理器服务（Dynamic Firewall Manager of Linux systems）是目前默认的防火墙管理工具，同时拥有命令行终端和图形化界面的配置工具，即使是对Linux命令并不熟悉的同学也能快速入门。相比于传统的防火墙管理工具还支持了动态更新技术并加入了“zone区域”的概念，简单来说就是为用户预先准备了几套防火墙策略集合（策略模板），然后可以根据生产场景的不同而选择合适的策略集合，实现了防火墙策略之间的快速切换。例如咱们有一台笔记本电脑每天都要在办公室、咖啡厅和家里使用，按常理推断最安全的应该是家里的内网，其次是公司办公室，最后是咖啡厅，如果需要在办公室内允许文件共享服务的请求流量、回到家中需要允许所有的服务，而在咖啡店则是除了上网外不允许任何其他请求，这样的需求应该是很常见的，在以前只能频繁的进行手动设置，而现在只需要预设好zone区域集合，然后轻轻点击一下就可以切换过去了上百条策略了，极大的提高了防火墙策略的应用效率，常见的zone区域名称及应用可见下表（默认为public）：

|区域|默认规则策略|
|:|
|trusted|允许所有的数据包。|
|home|拒绝流入的数据包，除非与输出流量数据包相关或是ssh,mdns,ipp-client,samba-client与dhcpv6-client服务则允许。|
|internal|等同于home区域|
|work|拒绝流入的数据包，除非与输出流量数据包相关或是ssh,ipp-client与dhcpv6-client服务则允许。|
|public|拒绝流入的数据包，除非与输出流量数据包相关或是ssh,dhcpv6-client服务则允许。|
|external|拒绝流入的数据包，除非与输出流量数据包相关或是ssh服务则允许。|
|dmz|拒绝流入的数据包，除非与输出流量数据包相关或是ssh服务则允许。|
|block|拒绝流入的数据包，除非与输出流量数据包相关。|
|drop|拒绝流入的数据包，除非与输出流量数据包相关。|

##### firewalld管理工具firewall-cmd
firewall-cmd命令是Firewalld动态防火墙管理器服务的命令行终端。它的参数一般都是以“长格式”来执行的。

|参数|作用|
|:|
|--get-default-zone|查询默认的区域名称。|
|--set-default-zone=<区域名称>|设置默认的区域，永久生效。|
|--get-zones|显示可用的区域。|
|--get-services|显示预先定义的服务。|
|--get-active-zones|显示当前正在使用的区域与网卡名称。|
|--add-source=|将来源于此IP或子网的流量导向指定的区域。|
|--remove-source=|不再将此IP或子网的流量导向某个指定区域。|
|--add-interface=<网卡名称>|将来自于该网卡的所有流量都导向某个指定区域。|
|--change-interface=<网卡名称>|将某个网卡与区域做关联。|
|--list-all|显示当前区域的网卡配置参数，资源，端口以及服务等信息。|
|--list-all-zones|	显示所有区域的网卡配置参数，资源，端口以及服务等信息。|
|--add-service=<服务名>|设置默认区域允许该服务的流量。|
|--add-port=<端口号/协议>|允许默认区域允许该端口的流量。|
|--remove-service=<服务名>|设置默认区域不再允许该服务的流量。|
|--remove-port=<端口号/协议>|允许默认区域不再允许该端口的流量。|
|--reload|让“永久生效”的配置规则立即生效，覆盖当前的。|

如同其他的防火墙策略配置工具一样，Firewalld服务对防火墙策略的配置默认是当前生效模式（RunTime），配置信息会随着计算机重启而失效，如果想要让配置的策略一直存在，那就要使用永久生效模式（Permanent）了，方法就是在正常的命令中加入--permanent参数就可以代表针对于永久生效模式的命令了。但这个永久生效模式也有一个“不近人情”的特点，就是对它设置的策略需要重启后才能自动生效，如果想让配置的策略立即生效的话需要手动执行一下--reload参数。

```bash
#查看默认zone
firewall-cmd --get-default-zone
#查看网卡在firewalld中的zone
firewall-cmd --get-zone-of-interface=eth0
#把Firewalld防火墙服务中网卡的默认区域修改为external，重启后再生效
firewall-cmd --permanent --zone=external --change-interface=eth0
firewall-cmd --permanent --get-zone-of-interface=eth0
#查询所有的zone
firewall-cmd --get-zones
#设置默认zone
firewall-cmd --set-default-zone=work
#启动/关闭Firewalld防火墙服务的应急状况模式，阻断一切网络连接（当远程控制服务器时请慎用。）
firewall-cmd --query-panic
firewall-cmd --panic-on
firewall-cmd --panic-off
#查询在public区域中的ssh服务请求流量是否被允许
firewall-cmd --zone=public --query-service=ssh
# 当前生效，立即生效，重启失效。
firewall-cmd --zone=public --add-service=ssh
# 永久生效，不立即生效，重启后永久生效。
firewall-cmd --permanent --zone=public --add-service=ssh
#把Firewalld防火墙服务中80的请求流量允许放行，但仅限当前生效
firewall-cmd --zone=public --add-port=80
#查看允许端口
firewall-cmd --zone=public --list-ports
# 删除
firewall-cmd --zone=public --remove-service=ssh
#把原本访问本机888端口号的请求流量转发到22端口号，要求当前和长期均有效
# 流量转发命令格式：firewall-cmd --permanent --zone=<区域> --add-forward-port=port=<源端口号>:proto=<协议>:toport=<目标端口号>:toaddr=<目标IP地址>
firewall-cmd --permanent --zone=public --add-forward-port=port=888:proto=tcp:toport=22:toaddr=192.168.1.5
firewall-cmd --reload
#在Firewalld防火墙服务中配置一条富规则，拒绝所有来自于192.168.1.0/24网段的用户访问本机ssh服务（22端口）
firewall-cmd --permanent --zone=public --add-rich-rule="rule family="ipv4" source address="192.168.1.0/24" service name="ssh" reject"
#
firewall-cmd --direct --add-rule ipv4 filter INPUT 0 -p tcp --dport 9000 -j ACCEPT
#立即生效
firewall-cmd --reload
```

#### tcp_wrappers
服务的访问控制列表

Tcp_wrappers是红帽RHEL7系统中默认已经启用的一款流量监控程序，它能够根据来访主机地址与本机目标服务程序做允许或拒绝操作。换句话说，Linux系统中其实有两个层面的防火墙，第一种是前面讲到的基于TCP/IP协议的流量过滤防护工具，而Tcp_wrappers服务则是能够对系统服务进行允许和禁止的防火墙，从而在更高层面保护了Linux系统的安全运行。控制列表文件修改后会立即生效，系统将会先检查允许策略规则文件（/etc/hosts.allow），如果匹配到相应的允许策略则直接放行请求，如果没有匹配则会去进一步匹配拒绝策略规则文件（/etc/hosts.deny）的内容，有匹配到相应的拒绝策略就会直接拒绝该请求流量，如果两个文件全都没有匹配到的话也会默认放行这次的请求流量。配置服务的参数并不复杂，如下表中就已经列出了几乎所有常见的情况：

|客户端类型|示例|满足示例的客户端列表|
|:|
|单一主机|192.168.1.5|IP地址为192.168.1.5的主机。|
|指定网段|192.168.1.|IP段为192.168.1.0/24的主机。|
|指定网段|192.168.1.0/255.255.255.0|IP段为192.168.1.0/24的主机。|
|指定DNS后缀|.baidu.com|所有DNS后缀为.baidu.com的主机|
|指定主机名称	www.baidu.com|主机名称为www.baidu.com的主机。|
|指定所有客户端|ALL|所有主机全部包括在内。|

在正式配置Tcp_wrappers服务前有两点原则必须要提前讲清楚，第一，在写禁止项目的时候一定要写上的是服务名称，而不是某种协议的名称，第二，推荐先来编写拒绝规则，这样可以比较直观的看到相应的效果。例如先来通过拒绝策略文件禁止下所有访问本机sshd服务的请求数据吧（无需修改原有的注释信息）。

应用层
/etc/hosts.allow
sshd : 192.168.1.
/etc/hosts.deny
sshd:*

优先级
默认先匹配hosts.allow

### 使用SSH服务管理远程主机
#### 配置网卡服务
##### nmtui
##### nmcli
```bash
nmcli connection show
```

创建网络会话

红帽RHEL7系统中的网卡服务已经支持了多会话技术了，这个功能非常类似于Firewalld防火墙服务中的zone区域技术，让用户能够在多个配置文件中快速切换。比如白天需要拿着笔记本电脑去公司上班，下午去咖啡厅休息一下，然后晚上再把笔记本电脑拿回到家中，如果在公司上班工作的时候是需要手工指定网卡IP地址，而晚上回家后用无线路由器是DHCP自动分配网卡IP地址，对传统网卡服务这样改来改去一定会很麻烦，但是使用了网卡会话功能就不一样了，咱们只需要在不同的使用环境中激活相应的会话服务，就能对网卡参数进行整体性的自动切换了。

可以使用nmcli命令将在公司上班时的网卡会话叫做company，而在家里的网卡会话叫做house，然后按照connection(会话),add(添加动作),con-name(会话名称),type(网卡类型),ifname(网卡名称)的格式来创建网卡会话就可以了，例如依次创建公司和家庭的网卡会话。

```bash
#company
nmcli connection add con-name company ifname eno16777736 autoconnect no type ethernet ip4 192.168.10.10/24 gw4 192.168.10.1
#house
nmcli connection add con-name house type ethernet ifname eno16777736
#切换网卡会话
nmcli connection up house
```

##### 绑定两块网卡
一般生产环境是必须要保证7*24小时不间断提供网络传输服务的，使用网卡绑定技术不仅能够提高网卡带宽的传输速率，还能在其中一块网卡出现故障时，依然能够保证网络正常使用。简单来说，假设咱们对两块网卡实施了绑定技术，这样在正常工作中它们会共同传输数据，使得网络传输的速度变得更快，但只要其中有一块网卡突然出现了故障，另外一块网卡便会在0.1秒内自动顶替上去，保证数据传输不会中断。

通过vim文本编辑器来配置网卡设备的绑定参数，网卡绑定的理论很类似于前面学习的RAID磁盘阵列组，咱们需要先逐个对参与网卡绑定的设备进行“初始设置”，这些原本独立的网卡设备不需要再有自己的IP地址等信息，让它们支持网卡绑定设备就可以了，然后还需要将绑定后的设备取名为bond0以及把IP地址等信息填写进去，这样当用户访问到相应服务的时候，实际上就是由这两块网卡设备共同的在提供服务。

```
vi /etc/sysconfig/network-scripts/ifcfg-eth0
TYPE=Ethernet
BOOTPROTO=none
ONBOOT=yes
USERCTL=no
DEVICE=eth0
MASTER=bond0
SLAVE=yes

vi /etc/sysconfig/network-scripts/ifcfg-eth1
TYPE=Ethernet
BOOTPROTO=none
ONBOOT=yes
USERCTL=no
DEVICE=eth1
MASTER=bond0
SLAVE=yes

vi /etc/sysconfig/network-scripts/ifcfg-bond0
TYPE=Ethernet
BOOTPROTO=none
ONBOOT=yes
USERCTL=no
DEVICE=bond0
IPADDR=192.168.1.5
PREFIX=24
DNS=192.168.1.1
NM_CONTROLLED=no
```

让内核支持网卡绑定驱动，常见的网卡绑定驱动模式有三种——mode0、mode1和mode6。比如对于一个提供NFS或者SAMBA服务的文件服务器来讲，如果只能提供百兆网络的最大传输速率，但同时下载用户又特别多的情况下，那么网络压力一定是极大的。再比如是一个提供iscsi服务的网络存储服务器，在生产环境中网卡的可靠性是极为重要的，这种情况下就必须同时能够保证网络的传输速率以及网络的安全性，因此比较好的选择就是mode6方案了，因为mode6平衡负载模式能够让两块网卡同时一起工作，当其中一块网卡出现故障后能自动备援，提供了可靠的网络传输保障，并且不需要交换机设备支援。

>mode0平衡负载模式:平时两块网卡均工作，且自动备援，采用交换机设备支援。
>mode1自动备援模式:平时只有一块网卡工作，故障后自动替换为另外的网卡。
>mode6平衡负载模式:平时两块网卡均工作，且自动备援，无须交换机设备支援。

使用vim文本编辑器来创建一个网卡绑定内核驱动文件，使得bond0网卡设备能够支持绑定技术（bonding），同时定义网卡绑定为mode6平衡负载模式，且当出现故障时自动切换时间为100毫秒。

```
vi /etc/modprobe.d/bond.conf
alias bond0 bonding
options bond0 miimon=100 mode=6
```

重启网络服务后网卡绑定操作即可顺利成功，正常情况下只有bond0网卡才会有IP地址等信息。

```bash
systemctl restart network
```

## 第十二节课（上） 第十二节课（下）
### 使用SSH服务管理远程主机
#### 远程控制服务
##### 配置sshd服务
SSH(Secure Shell)是一种能够提供安全远程登录会话的协议，也是目前远程管理Linux系统最首选的方式，因为传统的ftp或telnet服务是不安全的，它们会把帐号口令和数据资料等数据在网络中以明文的形式进行传送，这种数据传输方式很容易受到黑客“中间人”的嗅探攻击，轻则篡改了传输的数据信息，重则直接抓取到了服务器的帐号密码。

想要通过SSH协议来管理远程的Linux服务器系统，咱们需要来部署配置sshd服务程序，sshd是基于SSH协议开发的一款远程管理服务程序，不仅使用起来方便快捷，而且能够提供两种安全验证的方法——基于口令的安全验证，指的就是用帐号和密码验证登陆，基于密钥的安全验证，则是需要在本地生成密钥对，然后把公钥传送至服务端主机进行的公共密钥比较的验证方式，相比较来说更加的安全。

因为在Linux系统中的一切都是文件，因此要想在Linux系统中修改服务程序的运行参数，实际上也是在修改程序配置文件的过程，sshd服务的配置信息保存在/etc/ssh/sshd_config文件中，运维人员一般会把保存着最主要配置信息的文件称为主配置文件，而配置文件中有许多#(井号)开头的注释行，要想让这些配置参数能够生效，需要修改参数后再去掉前面的井号才行，sshd服务配置文件中重要的参数包括有：

|参数|作用|
|:|
|#Port 22|默认的sshd服务端口。|
|#ListenAddress 0.0.0.0|设定sshd服务端监听的IP地址。|
|#Protocol 2|SSH协议的版本号。|
|#HostKey /etc/ssh/ssh_host_key|SSH协议版本为1时，私钥存放的位置。|
|HostKey /etc/ssh/ssh_host_rsa_key	|SSH协议版本为2时，RSA私钥存放的位置。|
|#HostKey /etc/ssh/ssh_host_dsa_key|SSH协议版本为2时，DSA私钥存放的位置。|
|#PermitRootLogin yes|设定允许root用户直接登录。若要不允许root登录更改yes为no。|
|#StrictModes yes|当远程用户私钥改变时则直接拒绝连接。|
|#MaxAuthTries 6|最大密码尝试次数|
|#MaxSessions 10|最大终端数|
|#PasswordAuthentication yes|是否允许密码验证|
|#PermitEmptyPasswords no|是否允许空密码登陆（很不安全）|

一般的服务程序并不会在修改配置文件后就立即获取到了最新的运行参数，如果想让新的配置文件起效，则需要手动的重启一下服务程序才行，并且最好也能将这个服务程序加入到开机启动项中，这样使得下一次重启时sshd服务程序会自动运行。

```bash
systemctl restart sshd
systemctl enable sshd
```

###### 安全密钥验证
加密算法是对信息进行编码和解码的技术，它能够将原本可直接阅读的明文信息通过一定的加密算法译成密文形式。密钥即是密文的钥匙，包括有私钥和公钥之分，在日常工作中如果担心传送的数据被他人监听截获到，就可以在传送前先使用公钥进行加密处理，然后再进行数据传送，这样只有掌握私钥的用户才能解密这段数据，除此之外其他人即便截获了数据内容一般也很难再破译成明文信息。总结来说，日常生产环境中使用密码进行口令验证终归存在着被骇客暴力破解或嗅探截获的风险，如果正确的配置密钥验证方式，那么一定会让您的sshd服务程序更加的安全。

```bash
#在客户端主机中生成“密钥对”并把公钥传送到远程服务器中
ssh-keygen -t rsa -b 4096 -C "注释email" -f ovwane_rsa
#把客户端主机中生成好的公钥文件传送至远程主机
ssh-copy-id 192.168.1.5
#设置服务器主机只允许密钥验证，拒绝传统口令验证方式，记得修改配置文件后保存并重启sshd服务程序哦~
#禁止密码登录
sed -i 's/^PasswordAuthentication yes/PasswordAuthentication no/g' /etc/ssh/sshd_config
#允许密钥登录
sed -i 's/^#PubkeyAuthentication yes/PubkeyAuthentication yes/g' /etc/ssh/sshd_config
#重启sshd服务
systemctl restart sshd
```

#### 远程传输命令
Scp安全传输服务(Secure copy)是一种基于ssh协议的网络传输命令，scp不仅能够通过网络传送数据，而且所有的数据都将被加密。例如想把一些文件通过网络传送到其他主机上面，又恰巧两台主机都是Linux系统，这时就可以使用到scp命令轻松的达到目的啦~

scp命令用于在网络中安全的传输文件，格式为：“scp [参数] 本地文件 远程帐户@远程IP地址:远程目录”。

|参数|作用|
|:|
|-v|显示详细的连接进度|
|-P|指定远程主机的sshd端口号|
|-r|传送文件夹时请加此参数|
|-6|使用ipv6协议|

想要使用scp命令把文件从本地复制到远程主机，首先需要以绝对路径的形式写清本地文件的存放位置，对于要传送整个目录内所有数据的时候还需要额外添加-r参数递归操作，然后写上要传送到的远程服务器主机IP地址，默认会以当前本地登陆的用户身份来进行验证，如果想使用其他指定用户的身份进行校验，可使用用户名@主机地址的参数格式，最后需要在远程主机IP地址后以:（冒号）起始写上要传送到对方主机的那个目录中，把参数写正确并验证用户身份后即可完成传送工作。因为scp命令是基于ssh协议进行文件传送的，如果设置好了密钥验证，因此进行文件传送的话是连帐号密码都不需要的。

```bash
scp /root/file 192.168.1.5:/root
```

不仅如此，强大的scp命令还能够把远程主机的文件下载到本地主机，命令格式为scp [参数] 远程用户@远程IP地址:远程文件 本地目录，例如可以把远程主机中的系统版本信息文件下载过来，而无需登录到远程主机后再传送过来，省去了很多不必要的周折。

```bash
scp 192.168.1.5:/etc/resolv.conf /root
```

#### 不间断会话服务
大家学完了ssh服务后有没有发现一个很重要的事情——当远程连接的终端被关闭时，运行在对方服务器上的命令也会随之中断。简单来说，如果在执行打包文件的命令，或者是正在用脚本安装某个服务程序，通常中途是绝对不能关闭远程窗口或断开网络链接的，即便是网络不稳定的波动情况都有可能导致任务被中断，只能重新远程链接到服务器再重新开始任务。还有些时候正在执行打包文件的命令，又同时想用脚本来安装某个服务程序，这个时候因为打包文件的输出信息会占满了用户的屏幕界面，所以只能再额外打开一个远程控制窗口，久而久之了，难免会忘记那个远程窗口是做什么用的了。

Screen是一款由GNU开源计划开发的多视窗远程控制管理服务，简单来说就是为了解决上述情况中网络异常中断或同时控制多个远程窗口而设计的程序。Screen服务程序不仅能够解决上述问题，而且用户在使用过程中还可以同时在多个终端会话中自由切换，能够做到会话恢复——即便网络中断，也可让会话随时恢复，用户不会失去对命令终端的控制，多窗口——每个会话都是独立运行的，拥有各自独立的编码、输入输出和窗口缓存，会话共享——可以使多个用户从不同终端使用同一个会话，也可让他们看到完全相同的输出信息的。

##### screen
Screen服务程序在红帽RHEL7系统中并没有默认安装，因此需要配置yum仓库来安装这款软件。

需要rhel的安装光盘

```bash
mkdir -p /media/cdrom
mount /dev/cdrom /media/cdrom
#使用Vim文本编辑器创建Yum仓库的配置文件
cat << EOF >/etc/yum.repos.d/rhel7.repo
[rhel7]
name=rhel7
baseurl=file:///media/cdrom
enabled=1
gpgcheck=0
EOF

#现在就可以使用yum仓库来安装screen服务程序软件啦，出于对自然环境的保护意识，刘遄老师将对后面章节中出现的yum软件安装信息进行筛选——把重复性高与无意义的非必要信息省略，以来节省及避免不必要的纸张浪费，为后代不仅能享受科技之便捷，还能拥有自然之舒适献出一点努力。
yum install screen 
```

##### 管理远程会话
screen命令能做的事情非常多。

|参数|作用|
|:|
|-S|创建会话窗口|
|-d|把指定会话离线|
|-r|把指定会话恢复|
|-x|一次性恢复所有的会话|
|-ls|显示当前已有的会话|
|-wipe|把目前无法使用的会话删除等等功能|

```bash
#创建一个名称为backup的会话窗口，同学们可以留心观察下，当咱们敲下这条命令的一瞬间屏幕会快速闪动一下，这时就已经处在screen服务会话中了，在这个里面运行任何操作都会被后台记录下来。
screen -S backup
#在刚刚执行screen命令后调用系统的默认shell，所以执行命令后会立即返回一个提示符，虽然看起来跟刚刚没有变化，但实际上可以查看到当前的会话正在工作中
screen -ls
#切换会话
screen -r sessionname
#想要退出一个会话也十分简单，只需要向命令行中执行exit命令就可以达到目的
exit
#日常生产中其实并不是必须先创建会话之后再开始工作，可以直接使用screen命令执行要运行的命令，这样在命令中的一切操作也都会被记录下来，当命令执行结束后screen会话也会自动结束。
screen vim test.txt
```

当然如果咱们突然又想到了还有其他事情要做，也可以再多创建出几个会话窗口来一起使用，而如果这段时间内不会再使用某个会话窗口，可以把它设置为临时断开模式（detach），随后工作需要时再重新连接（attach）回来，这期间会话内的程序也会正常运行。

会话共享功能
screen命令不仅能够让使用者在极端情况下也不丢失对系统的远程控制，保证了生产环境远程工作的不间断性，而且还具有会话共享、分屏切割、会话锁定等等实用的功能。会话共享功能是一件很酷的事情，当多个用户同时用某个用户远程控制主机的时候，便可以把屏幕内容共享出来，也就是说每个人都可以看到相同的内容。

```bash
#要实现会话共享功能，需要先通过客户终端A远程ssh连接到服务器主机端，并创建一个会话窗口
screen -S sessionname
#然后开启第二台客户终端B远程ssh连接到服务器主机端，执行获取远程会话的命令即可，马上两台主机就能看到相同的内容了。
screen -x
```

### 使用Apache服务部署静态网站
#### 网站服务程序
最初在1970年前后，互联网技术仅仅是处在雏形阶段的一种构想，经过多年发展然后才慢慢逐步向非军方或科研部门外开始接入使用，虽然那时“互联网”的规模还不如现在局域网成熟，但依然为网络应用技术爆发式增长打下了扎实的基础。您最开始也应该是通过访问网站而正式接触到互联网的吧，而咱们平时访问的网站服务就是Web网络服务也叫WWW万维网(World Wide Web)，一般是指能够让用户通过浏览器访问到互联网中文档等资源的服务。如图10-1所示，Web网站服务是一种被动访问的服务程序，即只有接收到互联网中其他计算机发出的请求后才会响应，最终Web服务器会通过HTTP(超文本传输协议)或HTTPS（超文本安全传输协议）把指定文件传送到客户机的浏览器上。

目前能够提供WEB网络服务的程序有Apache、Nginx或IIS等等，在Windows系统中默认Web服务程序是IIS互联网信息服务(Internet Information Services)，这是一款图形化的网站管理工具，IIS程序不光能提供Web网站服务，还能够提供FTP、NMTP、SMTP等服务功能，但只能在Windows系统中使用，所以就不在咱们这本《Linux就该这么学》书籍的学习范围内了。2004年10月4日为俄罗斯知名门户站点而开发的Web服务程序Nginx出世了，Nginx程序作为一款轻量级的网站服务软件，因其稳定性和丰富的功能而快速占领服务器市场，但最最最被认可的还当属是低系统资源、占用内存少且并发能力强，目前国内如新浪、网易、腾讯等门户站均使用，这款服务程序刘遄老师将会在第20章节时为您讲解。Apache程序是目前拥有很高市场占有率的Web服务程序之一，其跨平台和安全性广泛被认可且拥有快速、可靠、简单的API扩展，如图10-2所示为Apache服务基金会的著名Logo，它的名字取自美国印第安人土著语，寓意着拥有高超的作战策略和无穷的耐性，在红帽RHEL5、6、7系统中一直作为着默认的Web服务程序而使用，并且也一直是红帽RHCSA和红帽RHCE的考试重点内容。Apache服务程序可以运行在Linux系统、Unix系统甚至是Windows系统中，支持基于IP、域名及端口号的虚拟主机功能、支持多种HTTP认证方式、集成有代理服务器模块、安全Socket层(SSL)、能够实时监视服务状态与定制日志消息，并有着各类丰富的模块支持。

安装Apache服务程序啦，需要注意使用yum命令安装软件时后面写的是服务程序的名字，而apache服务的软件包名称叫做httpd，直接执行yum install apache命令则是会报错误的。

```bash
#安装httpd
yum -y install httpd
#启动和开机自启动
systemctl start httpd
systemctl enable httpd
```

#### 配置服务文件参数
先慢着别激动!!刚刚学会的安装和运行只是学习httpd服务程序成功路上的一小步而已，对于Linux系统中服务的配置就是在修改其配置文件，因此还需要知道这些配置文件分别干什么用的，以及存放到了什么位置：
***

服务目录	/etc/httpd
主配置文件	/etc/httpd/conf/httpd.conf
网站数据目录	/var/www/html
访问日志	/var/log/httpd/access_log
错误日志	/var/log/httpd/error_log
***

主要需要讲解的是全局配置参数与区域配置参数的区别，全局配置参数顾名思义就是一种全局性的配置参数，这些全局配置参数对所有的子站点都是都是生效的，既保证了子站点的正常访问，还有效减少了频繁写入重复参数的工作量，而区域配置参数则是针对于每个独立子站点进行的特殊参数设置，在httpd服务程序主配置文件中最为常用的参数包括有：
***

ServerRoot	服务目录
ServerAdmin	管理员邮箱
User	运行服务的用户
Group	运行服务的用户组
ServerName	网站服务器的域名
DocumentRoot	网站数据目录
Listen	监听的IP地址与端口号
DirectoryIndex	默认的索引页页面
ErrorLog	错误日志文件
CustomLog	访问日志文件
Timeout	网页超时时间,默认为300秒.
Include	需要加载的其他文件
***

#### SELinux安全子系统
SELinux全称为Security-Enhanced Linux是美国国家安全局在Linux开源社区帮助下开发的一个MAC强制访问控制的安全子系统，在红帽RHEL7系统中启用SELinux技术的目的是为了让各个服务进程都受到约束，仅能获取到服务本应获取到的资源。例如您在自己的电脑上下载了一个美图软件，正在全神贯注得把自己P瘦点的时候，这款软件却在后台默默监听着浏览器中输入的密码信息，这显然不应该是这款美图软件本应做的事情（即便是访问电脑中的图片资源都情有可原），SELinux安全系统就是为了杜绝此类情况而设计的，它能够从多方面进行违法行为监控，首先是对服务进程进行功能限制，SELinux域限制技术让服务程序做不了出格的事情，其次还能够对文件进行资源限制，SELinux安全上下文让文件只能被所属于的服务程序所获取到。

刘遄老师经常会把SELinux域和SELinux安全上下文比喻成双管齐下，系统内的服务程序只能规规矩矩的拿到自己所应该获取的资源，这样即便黑客入侵了咱们的系统也毫无办法，但非常可惜的是由于SELinux服务比较复杂，配置起来的难度也较高，加之很多运维人员对这项技术理解不深，导致SELinux服务在很多服务器部署系统后就被直接禁用了，但这绝对不是明智的选择。SELinux服务有三种模式分别为：enforcing - 安全策略强制启用模式，将会拦截服务的不合法请求，permissive - 遇到服务越权访问只会发出警告而不强制拦截，disabled - 对于越权的行为不警告，也不拦截。

SELinux服务的主配置文件
/etc/selinux/config

SELinux服务的主配置文件定义的是默认运行状态，您也可以理解成是系统重启后的状态，并不是当前立即生效的，要想获得当前SELinux服务的运行模式可以使用getenforce命令。

```bash
#SELinux服务的主配置文件定义的是默认运行状态，您也可以理解成是系统重启后的状态，并不是当前立即生效的，要想获得当前SELinux服务的运行模式可以使用getenforce命令
getenforce
#为了确认是这个讨厌的SELinux服务在“捣乱”，咱们可以用setenforce [0|1]命令来修改下当前服务的运行模式（0为禁用，1为启用），但这种修改只是临时的，重启后就会失效
setenforce 0
```

在文件上面设置的SELinux安全上下文是由用户段、角色段以及类型段等等多个信息项目共同组成的，用户段中system_u代表系统进程身份，角色段object_r代表文件目录角色，类型段httpd_sys_content_t代表是网站服务系统文件。由于SELinux服务实在过于复杂，因此现在您只需要简单熟悉SELinux服务的作用就可以，现在这种情况的解决办法就是把当前网站目录/home/wwwroot的SELinux安全上下文修改为跟原始网站目录的一样就可以啦~

##### semanage
默认系统没有安装 semanage，安装semanage。

```bash
yum install -y policycoreutils-python
```

semanage命令用于查询与修改SELinux的安全上下文，格式为：“semanage [选项] [文件]”。

SELinux服务极大的增强了Linux系统的安全性，将用户权限牢牢地锁在笼子里，semanage命令不仅能够像传统chcon命令一样对文件、目录进行策略设置，而且还能够对网络端口、消息接口等等进行管理，这些新特性咱们会在本章中慢慢感受。在使用semanage命令的时候有几个比较常用的参数，-l参数用于查询、-a参数用于添加、-m参数用于修改、-d参数用于删除等等，例如可以向新的网站数据目录中新增加一条SELinux安全上下文，让这个目录以及里面的所有文件能够被httpd服务程序所获取到：

```bash
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/*
#使用restorecon命令来让刚刚设置的SELinux安全上下文立即生效
restorecon -Rv /home/wwwroot/
```

#### 个人用户主页功能
如果想为系统中每位用户都建立一个独立的网站，通常的方法只能是基于虚拟网站主机功能来部署出多个网站，但这未免会让管理员感觉到很麻烦，而且在用户管理自己网站的时候还可能碰到种种权限的限制，产生出很多不必要的工作。其实如果只是想为每位用户建立独立的网站，不妨试试httpd服务程序提供的个人用户主机功能吧，这项功能可以让系统内所有的用户在自己的家目录中管理个人的网站，访问起来也非常容易。

httpd服务程序中的个人用户主页功能默认是没有被开启的，需要编辑下面的配置文件，然后第17行左右的UserDir disabled参数前面加上#（井号），这样代表让httpd服务程序开启个人用户主页功能，同时再把第23行左右的UserDir public_html参数前面的#（井号）去掉，该参数代表网站数据在用户家目录中的保存目录名称(即public_html目录)，最后一定要记得保存下哦。

```
vi /etc/httpd/conf.d/userdir.conf

# UserDir disabled
UserDir public_html
```

在用户家目录中建立用于保存网站数据的目录及首页文件，另外需要把家目录的权限修改为755，保证其他人也可以有权限读取里面的网页内容。

```bash
su - linuxprobe
mkdir public_html
echo "This is linuxprobe's website" > public_html/index.html
chmod -Rf 755 /home/linuxprobe
```

重新启动httpd服务程序，打开浏览器中输入网址后加上~用户名，理论上来讲就可以看到用户的个人主页网站，不过不出所料的依然是报错页面，一定还是SELinux在跟咱们“捣鬼”啦。

首先需要先思考下这次报错的原因是什么，httpd服务程序提供个人用户主页功能的网站数据目录本身就应该是存放到每个用户家目录中的，所以对于文件上面的SELinux安全上下文应该是不需要修改，但除此之外还有个Linux域，这个是负责管理让服务程序不能做违规的动作，只能本本分分的为用户服务，但突然开启的这项个人用户主页功能到底有没有被默认允许呢？咱们可以用getsebool命令来查询并过滤出所有跟http服务相关的安全策略，off为禁止状态，on为允许状态。

```bash
 getsebool -a | grep http
```

对于如此多的SELinux域功能策略，实在没有必要逐个去理解它们，只要能通过名字大致猜测出相关的策略作用就足够了。比如想开启httpd服务的个人用户主页功能，那么用到的SELinux策略应该是httpd_enable_homedirs吧？大致确定后就可以用setsebool命令来修改SELinux策略中各项规则的布尔值了，同学们一定要记得加上-P参数让修改过后的SELinux布尔值策略项目永久生效这样操作后也会是立即生效的，随后刷新下网页看看效果吧。

```bash
setsebool -P httpd_enable_homedirs=on
```

当把个人用户网站功能实现之后也会遇到一个很尴尬的显示——或许用户们并不希望直接就把网页内容显示出来，或者只想让部分读者看到里面的内容，这时就可以给网站上面加上口令验证功能啦，给网页内容增加一道安全防护吧。

```bash
#需要先用htpasswd命令来生成密码数据库，-c参数代表第一次生成的意思，后面再分别追加上要生成到哪个文件中，以及验证要用到的用户名称即可（该用户不必是系统中已有的帐户）
htpasswd -c /etc/httpd/passwd linuxprobe

#编辑个人用户主页功能的配置文件
vim /etc/httpd/conf.d/userdir.conf
<Directory "/home/*/public_html">
 AllowOverride all
#刚刚生成出来的密码验证文件保存路径
 authuserfile "/etc/httpd/passwd"
#当用户尝试访问个人用户网站时的提示信息
 authname "My privately website"
 authtype basic
#用户进行帐号口令登陆时需要验证的用户名称
 require user linuxprobe
</Directory>

#重启服务
systemctl restart httpd
```

#### 虚拟网站主机功能
如果在每台Linux系统的服务器上面只能运行着一个网站，那么人气低流量小的草根站长就要反而承担着高昂的服务器租用费了，这显然也会造成很大硬件资源浪费，于是在VPS与云计算技术诞生以前，IDC服务供应商们为了能够让服务器资源更充分被合理的使用，也为了降低购买门槛，于是便纷纷使用了虚拟主机功能。虚拟主机就是把一台运行着的物理服务器上面分割出多个“虚拟的服务器”，但这项技术不能够实现目前云主机技术的硬件资源隔离，而是让这些网站共同使用服务器的硬件资源，一般供应商只会去限制硬盘的使用空间大小，这种服务器资源供应方式目前还有企业在使用着。Apache的虚拟主机功能是服务器基于用户请求的不同IP地址、主机域名或端口号实现提供多个网站同时为外部提供访问服务的技术。

##### 基于IP地址
当一台服务器上面拥有多个IP地址，用户通过请求访问不同的IP地址时会取得不同的网站页面资源，另外让每个网站都拥有一个独立IP地址对搜索引擎SEO优化也有很大的好处，因此这种虚拟主机的提供方式不仅是最常见的，也是更受站长朋友欢迎的。

分别在/home/wwwroot中创建三个用于保存不同网站数据的目录，并向其中分别写入网站的首页文件，每个首页文件中应有明确区分不同网站内容的字样信息，方便咱们稍后能更直观的检查效果。

```bash
mkdir -p /home/wwwroot/10
mkdir -p /home/wwwroot/20
mkdir -p /home/wwwroot/30
echo "IP:192.168.10.10" > /home/wwwroot/10/index.html
echo "IP:192.168.10.20" > /home/wwwroot/20/index.html
echo "IP:192.168.10.30" > /home/wwwroot/30/index.html
```

vim /etc/httpd/conf/httpd.conf

```
<VirtualHost 192.168.10.10>
DocumentRoot /home/wwwroot/10
ServerName www.linuxprobe.com
<Directory /home/wwwroot/10 >
AllowOverride None
Require all granted
</Directory>
</VirtualHost>
<VirtualHost 192.168.10.20>
DocumentRoot /home/wwwroot/20
ServerName bbs.linuxprobe.com
<Directory /home/wwwroot/20 >
AllowOverride None
Require all granted
</Directory>
</VirtualHost>
<VirtualHost 192.168.10.30>
DocumentRoot /home/wwwroot/30
ServerName tech.linuxprobe.com
<Directory /home/wwwroot/30 >
AllowOverride None
Require all granted
</Directory>
</VirtualHost>
```

设置SELinux

```bash
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/10
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/10/*
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/20
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/20/*
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/30
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/30/*

restorecon -Rv /home/wwwroot
```

##### 基于主机域名
当服务器无法为每个网站都分配一个独立IP地址的时候，可以试试让apache自动识别用户请求的域名网址，从而根据这些不同的域名请求来传输不同的网页内容。这种情况配置起来更加的简单，生产环境中只需要保证服务器上面有一个可用的IP地址（192.168.1.5）就可以了。

分别在/home/wwwroot中创建三个用于保存不同网站数据的目录，并向其中分别写入网站的首页文件，每个首页文件中应有明确区分不同网站内容的字样信息，方便咱们一会更直观的检查效果：

```bash
mkdir -p /home/wwwroot/www
mkdir -p /home/wwwroot/bbs
mkdir -p /home/wwwroot/tech
echo "WWW.linuxprobe.com" > /home/wwwroot/www/index.html
echo "BBS.linuxprobe.com" > /home/wwwroot/bbs/index.html
echo "TECH.linuxprobe.com" > /home/wwwroot/tech/index.html
```

在httpd服务的配置文件中大约113行处，分别追加写入三个基于主机名的虚拟主机网站参数，保存退出文件后记得要重启httpd服务才能生效哦：
vim /etc/httpd/conf/httpd.conf

```
<VirtualHost 192.168.1.5>
DocumentRoot "/home/wwwroot/www"
ServerName "www.linuxprobe.com"
<Directory "/home/wwwroot/www">
AllowOverride None
Require all granted
</directory>
</VirtualHost>
<VirtualHost 192.168.1.5>
DocumentRoot "/home/wwwroot/bbs"
ServerName "bbs.linuxprobe.com"
<Directory "/home/wwwroot/bbs">
AllowOverride None
Require all granted
</Directory>
</VirtualHost>
<VirtualHost 192.168.1.5>
DocumentRoot "/home/wwwroot/tech"
ServerName "tech.linuxprobe.com"
<Directory "/home/wwwroot/tech">
AllowOverride None
Require all granted
</directory>
</VirtualHost>
```

因为当前的网站数据目录还是在/home/wwwroot中，因此还是必须要把网站数据目录文件上面的SELinux安全上下文设置好，让文件上面的SELinux安全上下文与网站服务功能相吻合，最后还是要记得用restorecon命令让新配置的SELinux安全上下文立即生效呢，这样虚拟主机网站就可以立即被访问到了。

```bash
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/www
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/www/*
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/bbs
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/bbs/*
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/tech
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/tech/*
restorecon -Rv /home/wwwroot
```

##### 基于端口号
基于端口号的虚拟主机功能可以让用户通过访问服务器上面指定的端口号来找到想要访问的网站资源，而用apache配置虚拟主机功能中最复杂的也莫过于是基于端口号的了，因为不光需要考虑到httpd服务程序的配置因素，还需要考虑到SELinux服务对于新开设端口号的监控，占用服务器中80、443、8080类似的端口号是网站服务比较合理的请求，但再去占用其他的端口号就会遭受到SELinux服务的限制了，因此接下来的实验中既要考虑到文件上面SELinux安全上下文的限制，还要考虑到SELinux域对httpd网站服务程序功能的管控。

分别在/home/wwwroot中创建两个用于保存不同网站数据的目录，并向其中分别写入网站的首页文件，每个首页文件中应有明确区分不同网站内容的字样信息，方便稍后能更直观的检查效果。

```
mkdir -p /home/wwwroot/6111
mkdir -p /home/wwwroot/6222
echo "port:6111" > /home/wwwroot/6111/index.html
echo "port:6222" > /home/wwwroot/6222/index.html
```

在httpd服务的配置文件中大约43行后追加上监听6111和6222端口号的参数：

vim /etc/httpd/conf/httpd.conf 

```
#Listen 12.34.56.78:80
Listen 80
Listen 6111
Listen 6222
```

在httpd服务的配置文件中大约114行处，分别追加写入两个基于端口号的虚拟主机网站参数，保存退出文件后记得要重启httpd服务才能生效哦：

vim /etc/httpd/conf/httpd.conf

```
<VirtualHost 192.168.10.10:6111>
DocumentRoot "/home/wwwroot/6111"
ServerName www.linuxprobe.com
<Directory "/home/wwwroot/6111">
AllowOverride None
Require all granted
</Directory>
</VirtualHost>
<VirtualHost 192.168.10.10:6222>
DocumentRoot "/home/wwwroot/6222"
ServerName bbs.linuxprobe.com
<Directory "/home/wwwroot/6222">
AllowOverride None
Require all granted
</Directory>
</VirtualHost>
```

因为把网站数据目录存放在了/home/wwwroot中，因此还是必须要把网站数据目录文件上面的SELinux安全上下文设置好，让文件上面的SELinux安全上下文与网站服务功能相吻合，最后还是要记得用restorecon命令让新配置的SELinux安全上下文立即生效呢：

```bash
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/6111
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/6111/*
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/6222
semanage fcontext -a -t httpd_sys_content_t /home/wwwroot/6222/*
restorecon -Rv /home/wwwroot/
```

什么??!!在把httpd服务程序和SELinux安全上下文都配置妥当后，重启服务为什么会竟然出现报错信息呢？这是因为SELinux服务检测到6111和6222端口原本不属于apache应该需要的服务资源，但现在却被以httpd服务程序的名义监听使用了，便会直接拒绝掉了，咱们可以用semanage命令查询并过滤出所有与http协议相关的端口号SElinux允许列表。

```bash
semanage port -l | grep http
```

SELinux允许http协议使用的端口号中默认没有包含咱们的6111和6222，因此需要手动的添加进去就可以了，操作会立即生效，且重启过后依然有效，因此设置后再重启一下httpd服务程序就能看到网页内容了。

```bash
semanage port -a -t http_port_t -p tcp 6111
semanage port -a -t http_port_t -p tcp 6222
semanage port -l| grep http
systemctl restart httpd
```

#### Apache的访问控制
网站访问控制是可以基于来源主机名、IP地址或客户端浏览器特征等信息元素进行的网页资源控制，通过Allow或Deny指令实现允许或禁止某个主机访问服务器网站资源，其中Order指令用于定义Allow或Deny指令起作用的顺序，匹配原则是按顺序匹配规则并执行，若为匹配成功则执行后面的默认指令，比如说Order Allow,Deny代表先将客户端与允许规则进行对比，若匹配成功则允许请求，反而则会直接拒绝访问请求。

例如可以先在服务端网站数据目录中新建一个子目录，并向目录中写入一段内容的网页文件：

```
mkdir /var/www/html/server
echo "Successful" > /var/www/html/server/index.html
```

打开httpd服务程序的配置文件找到大约第129行左右，追加以下限制客户端访问的规则，含义是匹配所有浏览器为火狐（Firefox）的主机并允许他们访问，而除此之外的所有用户请求都将被拒绝。

```
vim /etc/httpd/conf/httpd.conf
<Directory "/var/www/html/server">
 SetEnvIf User-Agent "Firefox" ff=1
 Order allow,deny
 Allow from env=ff
 </Directory>

systemctl restart httpd
```

除了匹配客户端的浏览器标识，咱们还可以通过匹配客户端的来源IP地址进行访问控制，例如想只允许来自于192.168.10.20的主机访问Web网站资源，那么就可以把刚刚的参数修改成如下所示，这样重启httpd服务程序后再用本机（服务端IP地址为192.168.10.10）来访问网站的子目录就会提示访问被拒绝了，很有意思吧~

```bash
vim /etc/httpd/conf/httpd.conf
<Directory "/var/www/html/server">
 Order allow,deny  
 Allow from 192.168.10.20
 Order allow,deny
 Allow from env=ie
</Directory>
systemctl restart httpd
```

## 第十三节课
### 使用Vsftpd服务传输文件
文件传输协议

一般来讲，人们将电脑联网的首要需求就是获取资料，而文件传输是其中非常重要的方式之一，21世纪的互联网是由几千万台个人电脑、工作站、小型机、大型机等等不同型号、架构的物理设备共同组成的，即便是个人电脑上也可能会装有诸如Linux、Windows、UNIX、DOS等等不同的操作系统，所以为了能够在如此复杂多样的操作设备之间解决文件传输问题，于是便有了统一的FTP文件传输协议（File Transfer Protocol），这是一种能够让使用者在互联网中上传、下载文件的传输协议。很多同学在大学期间只知道FTP协议使用了21端口号，但实际上FTP文件传输协议默认占用了20、21两个端口号，20端口号用于进行数据传输，21端口号用于接受客户端执行的相关FTP命令与参数，FTP服务端普遍更多的应用于内网中，具有易于搭建、方便管理的特点，并且可以借助FTP客户端工具还可以轻松实现文件的多点下载和断点续传技术。

FTP服务器就是支持FTP传输协议的主机，与大多数服务程序一样，要想完成文件传输则需要FTP服务端和客户端的配合才行，用户可以通过客户端向FTP服务端发送指令参数，FTP服务端从而会依据接受到的命令作出相应动作，比如显示执行结果或把文件传输到客户端主机上，FTP协议的传输有两种不同的模式，主动模式是让FTP服务端主动向客户端发起链接请求，而被动模式则是让FTP服务端等待客户端的链接请求，默认情况下被动模式，咱们在第8章的防火墙课程中学习过，防火墙一般更多的是过滤从外网到内网的流量数据，因此有些时候必须改成主动模式才可以传输。

Vsftpd是一款运行在Linux操作系统上面的FTP服务端程序，Very Secure FTP Daemon顾名思义就是非常安全的FTP传输程序，vsftpd服务程序不仅完全开源且免费，而且具有很高的安全性、传输速率、支持虚拟用户验证功能等等其他FTP服务端程序所不具备的特点。

```bash
yum -y install vsftpd
```

vsftpd服务程序的主配置文件(/etc/vsftpd/vsftpd.conf)中参数总共有123行左右，但大多数都是以#（井号）开头的注释信息，咱们可以用grep命令的-v参数来过滤并反选出没有包含#（井号）的参数行，也就是把所有的注释信息都过滤掉，这样再通过输出重定向符写会到原始的主配置文件名称中即可。

```bash
mv /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf_bak
grep -v "#" /etc/vsftpd/vsftpd.conf_bak > /etc/vsftpd/vsftpd.conf
cat /etc/vsftpd/vsftpd.conf
```

vsftpd服务程序的主配置文件中常用的参数及作用介绍

|参数|作用|
|:|
|listen=[YES|NO]|是否以独立运行的方式监听服务。|
|listen_address=IP地址|设置要监听的IP地址。|
|listen_port=21|设置FTP服务的监听端口。|
|download_enable＝[YES|NO]|是否允许下载文件。|
|userlist_enable=[YES|NO]|是否启用“禁止登陆用户名单”。|
|userlist_deny=[YES|NO]|是否启用“禁止登陆用户名单”。|
|max_clients=0|最大客户端连接数，0为不限制。|
|max_per_ip=0|同一IP地址最大连接数，0为不限制。|
|anonymous_enable=[YES|NO]|是否允许匿名用户访问。|
|anon_upload_enable=[YES|NO]|是否允许匿名用户上传文件。|
|anon_umask=022|匿名用户上传文件的umask值。|
|anon_root=/var/ftp|匿名用户的FTP根目录。|
|anon_mkdir_write_enable=[YES|NO]|是否允许匿名用户创建目录。|
|anon_other_write_enable=[YES|NO]|是否开放匿名用户其他写入权限。|
|anon_max_rate=0|匿名用户最大传输速率(字节)，0为不限制。|
|local_enable=[YES|NO]|是否允许本地用户登陆FTP。|
|local_umask=022|本地用户上传文件的umask值。|
|local_root=/var/ftp|本地用户的FTP根目录。|
|chroot_local_user=[YES|NO]|是否将用户权限禁锢在FTP目录，更加的安全。|
|local_max_rate=0|本地用户最大传输速率(字节)，0为不限制。|

#### Vsftpd服务程序
vsftpd作为更加安全的FTP文件传输协议的服务程序，可以让用户分别通过匿名开放、本地用户和虚拟用户三种身份验证方式来登陆到FTP服务器上面。匿名开放是一种最不安全的验证模式，任何人都可以无需密码验证就登陆到FTP服务端主机。本地用户是通过Linux系统本地的帐号密码信息进行的验证模式，这种模式相比较匿名开放模式来说比较安全，配置起来也十分简单，不过如果被骇客暴力破解出FTP帐号信息，也就可以通过这个口令登陆到服务器系统中了，从而完全控制整台服务器主机。最后的虚拟用户是相比较最为安全的验证模式，需要为FTP传输服务单独建立用户数据库文件，虚拟出用来口令验证的帐户信息，这些帐号是在服务器系统中不存在的，仅供FTP传输服务做验证使用，因此这样即便骇客破解出了帐号口令密码后也无法登录到服务器主机上面，有效的降低了破坏范围和影响。

ftp命令是用来在命令行终端中对ftp传输服务进行控制连接的客户端工具，需要先手动的安装一下这个ftp客户端工具，以便于接下来的实验中校验效果。

```bash
yum install -y ftp
```

#####  匿名访问模式
vsftpd服务程序中匿名开放是一种最不安全的验证模式，任何人都可以无需密码验证就登陆到FTP服务端主机，这种模式一般只用来保存不重要的公开文件，尤其是在生产环境中更要注意不放敏感文件，当然也非常推荐用第8章中学习的防火墙管理工具（例如Tcp_wrappers服务程序）把vsftpd服务程序的允许访问主机范围设置为企业内网，这样还算能够保证基本的安全性。

vsftpd服务程序默认已经开启了匿名访问模式，需要做的就是进一步允许匿名用户的上传、下载文件的权限，以及让匿名用户能够创建、删除、更名文件的权限，这些权限对于匿名用户来讲非常的危险，咱们只是为了练习Linux系统中vsftpd服务程序的配置能力，十分不推荐在生产环境中使用，匿名用户的权限参数及介绍：

|参数|作用|
|:|
|anonymous_enable=YES|允许匿名访问模式。|
|anon_umask=022|匿名用户上传文件的umask值。|
|anon_upload_enable=YES|允许匿名用户上传文件|
|anon_mkdir_write_enable=YES|允许匿名用户创建目录|
|anon_other_write_enable=YES|允许匿名用户修改目录名或删除目录|

```
vim /etc/vsftpd/vsftpd.conf
anonymous_enable=YES
anon_umask=022
anon_upload_enable=YES
anon_mkdir_write_enable=YES
anon_other_write_enable=YES
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
```

确认参数填写正确后保存并退出vsftpd服务程序的主配置文件，还需要重启vsftpd服务程序来让新的配置服务参数生效。

```bash
systemctl restart vsftpd
systemctl enable vsftpd
```

/var/ftp 权限问题

SELinux问题

此时再次使用ftp命令登入到FTP服务器主机后依然会提示写入操作失败，但细心的同学一定发现报错信息已经产生了变化，在刚刚没有写入权限的时候提示说权限拒绝（Permission denied.）所以怀疑是权限的问题，但现在是提示创建目录的操作失败（Create directory operation failed.）那么同学们应该也能马上意识到是SELinux服务在限制这个操作了吧，查看下所有与ftp相关的SELinux域策略有那些吧

```bash
getsebool -a | grep ftp
```

根据策略的名称和经验可以猜测出是哪一条规则策略，在设置的时候请记得使用-P参数来让配置过的策略永久生效，保证在服务器重启后依然能够顺利写入文件，大家可以分别尝试下创建目录文件、对文件进行改名以及删除目录文件等等操作。

```bash
setsebool -P ftpd_full_access=on
```

##### 本地用户模式
本地用户模式是通过Linux系统本地的帐号密码信息进行的验证方式，这种模式相比较匿名开放模式来说比较安全，不过如果被骇客暴力破解出FTP帐号信息，也就可以通过这个口令登陆到服务器系统中了，从而完全控制整台服务器主机。本地用户模式配置起来十分简单，而且既然本地用户模式确实要比匿名开放模式更加的安全，因此推荐既然开启了本地用户模式，就把匿名开放模式给关闭了吧~对本地用户模式需要使用的权限参数及介绍如下表：

|参数|作用|
|:|
|anonymous_enable=NO|禁止匿名访问模式。|
|local_enable=YES|允许本地用户模式。|
|write_enable=YES|设置可写入权限。|
|local_umask=022|本地用户模式创建文件的umask值。|
|userlist_deny=YES|参数值为YES即禁止名单中的用户，参数值为NO则代表仅允许名单中的用户。|
|userlist_enable=YES|允许“禁止登陆名单”，名单文件为ftpusers与user_list。|

```
vim /etc/vsftpd/vsftpd.conf
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
```

确认参数信息已经填写正确就可以保存退出了，要想让新的配置参数生效还要记得重启一下vsftpd服务程序，并且在刚刚实验后还原了虚拟机的同学还请记得再把配置的服务加入到开机启动项中，让vsftpd服务程序在重启后依然能够正常使用。

```bash
systemctl restart vsftpd
systemctl enable vsftpd
```

在输入root管理员用户的密码前就已经提示被拒绝了，看来有什么东西再禁止着用户登陆，这是因为vsftpd服务程序目录中默认存在着两个文件（ftpusers或user_list），这两个文件叫做禁止用户名单。只要里面写有某个用户的名字，那么就不再允许这个用户登陆到FTP服务器上面。

```
cat /etc/vsftpd/user_list 
cat /etc/vsftpd/ftpusers 
```

果然如此，vsftpd服务程序为了保证服务器的安全性而默认禁止了超级管理员和大多数系统用户的登陆行为，这样可以有效的避免骇客通过FTP服务对超级管理员密码的暴力破解行为，如果您在生产环境中确认使用root超级管理员帐号不会对系统产生安全影响，只需要按照上面的提示删除掉root用户名即可。

本地用户模式登陆后默认会进入该用户的家目录中，也就是说当前用户登陆后应该进入的是/home/linuxprobe目录中，而该目录的默认所有者、所有组都是该用户自己，不存在写入权限不足的情况，因此这次提示的操作被拒绝，就是因为刚刚还原过虚拟机系统到最初始的状态了，需要再来开启一下SELinux域中对ftp服务的允许策略就可以了。

```
getsebool -a | grep ftp
setsebool -P ftpd_full_access=on
```

练习实验课程和生产环境中对SELinux域策略的设置中切记加入-P参数，否则服务器在重启后就会依然按照原有的策略进行控制，导致配置过的服务又不能使用了。

##### 虚拟用户模式
最后要学习的虚拟用户模式是一种相比较来说最为安全的验证方式，需要为FTP传输服务单独建立用户数据库文件，虚拟出用来口令验证的帐户信息，这些帐号是在服务器系统中不存在的，仅供FTP传输服务做验证使用，因此这样即便骇客破解出了帐号口令密码后也无法登录到服务器主机上面，有效的降低了破坏范围和影响。所以只要在配置妥当合理的情况下，虚拟用户模式要比前两种验证方式更加的安全，同时配置的流程也稍微会复杂一些。

第1步：创建用于进行FTP验证的帐号密码数据库文件，单数行为账户名，偶数行为密码，例如分别创建出张三和李四两个用户，密码均为redhat：

```
cd /etc/vsftpd/

vim vuser.list
zhangsan
redhat
lisi
redhat
```

但明文信息既不安全，也不能让vsftpd服务程序直接读取，因此需要使用db_load命令用HASH算法将这个原始的明文信息文件转换成数据库文件，并且再把数据库文件权限调小一些（避免其他人能看到数据库文件的内容），然后再把原始的明文信息文件删除掉。

```
db_load -T -t hash -f vuser.list vuser.db
file vuser.db
chmod 600 vuser.db
rm -f vuser.list
```

第2步：创建用于FTP服务存储文件的根目录以及虚拟用户映射的系统本地用户，FTP服务存储文件的根目录指的是当虚拟用户登陆后默认所在的位置，但在Linux系统中的每一个文件都是有所有者和所有组属性的，例如用张三帐户创建了一个新文件，但是张三这个用户在系统中是找不到的，就会导致Linux系统中这个文件权限出现错误。因此还需要再创建一个用来让虚拟用户映射的系统本地用户，简单来说就是让虚拟用户默认登陆到这个本地用户的家目录中，创建的文件属性也都归属于这个本地用户，避免本地Linux系统无法处理这种虚拟用户创建的文件属性权限。为了方便管理FTP资料，可以把这个用于虚拟用户映射的系统本地用户的家目录设置到/var目录中（因为该目录是用来存放经常发生改变的数据），并且为了安全起见把这个系统本地用户的终端设置成不允许登陆，这不会影响虚拟用户的使用，还可以有效避免骇客通过该用户登陆到服务器主机上面。

```
useradd -d /var/ftproot -s /sbin/nologin virtual
ls -ld /var/ftproot/
chmod -Rf 755 /var/ftproot/
```

第3步：建立用于支持虚拟用户的PAM认证文件，PAM可插拔认证模块(Pluggable Authentication Modules)是一种认证机制，通过一些动态链接库和统一的API把系统提供的服务与认证方式分开，使得系统管理员可以根据需求灵活的调整服务程序的不同认证方式。通俗来讲PAM是一组安全机制的模块(插件)，让系统管理员可以轻易的调整服务程序的认证方式，而可以不必对应用程序做任何的修改，易用性很强。PAM采取了分层设计的思想——应用程序层、应用接口层、鉴别模块层。

新建一个用于虚拟用户验证的PAM文件替换掉原始的PAM文件，其中PAM文件内的db=参数为刚刚用db_load生成出的账户密码数据库文件的路径，但不用写后缀：

```
vim /etc/pam.d/vsftpd.vu
auth       required     pam_userdb.so db=/etc/vsftpd/vuser
account    required     pam_userdb.so db=/etc/vsftpd/vuser
```

第4步：在vsftpd服务程序主配置文件中修改PAM支持文件，PAM API作为应用程序层与鉴别模块层的连接纽带，让应用程序可以根据需求灵活的在其中插入所需的鉴别功能模块，当应用程序需要PAM认证时，一般在应用程序中定义负责其认证的PAM配置文件，真正灵活的实现了认证功能。例如在vsftpd服务程序主配置文件中默认就写有参数pam_service_name=vsftpd，表示登录FTP服务器时是根据/etc/pam.d/vsftpd的文件内容进行安全认证的，现在需要做的就是把vsftpd主配置文件中原先的PAM认证文件vsftpd修改成咱们自建的vsftpd.vu即可，需要使用到的参数及介绍有：

|参数|作用|
|:|
|anonymous_enable=NO|禁止匿名开放模式。|
|local_enable=YES|允许本地用户模式。|
|guest_enable=YES|开启虚拟用户模式。|
|guest_username=virtual|指定虚拟用户帐号。|
|pam_service_name=vsftpd.vu|指定pam文件。|
|allow_writeable_chroot=YES|允许禁锢的FTP根目录可写而不拒绝用户登入请求。|

```
vim /etc/vsftpd/vsftpd.conf
anonymous_enable=NO
local_enable=YES
guest_enable=YES
guest_username=virtual
allow_writeable_chroot=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd.vu
userlist_enable=YES
tcp_wrappers=YES
```

第5步：为虚拟用户设置不同的权限，虽然张三和李四两个帐户都是用于FTP服务验证的虚拟帐户，但也想在他们之间区别对待。比如只允许张三用户能够上传、创建、修改、查看、删除文件，而李四只能查看文件，这其实也是可以让vsftpd服务程序实现的，咱们只需要新建一个目录，在里面分别创建两个以张三和李四命名的文件，其中在张三命名的文件中写入相关允许的权限（使用匿名用户的参数）：

```
mkdir /etc/vsftpd/vusers_dir/
cd /etc/vsftpd/vusers_dir/
touch lisi

vim zhangsan
anon_upload_enable=YES
anon_mkdir_write_enable=YES
anon_other_write_enable=YES
```

然后再次修改下vsftpd主配置文件，添加user_config_dir参数来定义这两个虚拟用户不同权限的配置文件所存放的路径即可，为了让刚刚配置的服务程序新参数立即生效，咱们需要把vsftpd再来重新启动一下，并加入到开机启动项中：

```
vim /etc/vsftpd/vsftpd.conf
anonymous_enable=NO
local_enable=YES
guest_enable=YES
guest_username=virtual
allow_writeable_chroot=YES
write_enable=YES
local_umask=022
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd.vu
userlist_enable=YES
tcp_wrappers=YES
user_config_dir=/etc/vsftpd/vusers_dir
```

```
systemctl restart vsftpd
systemctl enable vsftpd
```

第6步：设置SELinux域允许策略后尝试使用虚拟用户模式登陆FTP服务器，同学们有了刚刚两个验证模式的配置经验后一定能猜到SELinux会继续来“捣乱”，所以先按照刚刚的步骤开启SELinux域的允许策略吧，以免再次出现类似创建目录的操作失败的操作：

```
getsebool -a | grep ftp
setsebool -P ftpd_full_access=on
```

此时不仅能够使用虚拟用户模式登陆到FTP服务器中了，还可以分别使用张三和李四用户检验下他们不同的权限，有没有特别好玩呢~当然读者们在工作中要学会灵活的搭配参数，不要完全按照实验操作而脱离客户需求。

### TFTP简单文件传输协议
TFTP简单文件传输协议是一种基于TCP/IP协议用于在客户端和服务器之间进行简单文件传输的协议——即提供不复杂、开销不大的文件传输服务，亦可把其当作是FTP协议的简化版本。TFTP并不能提供类似于FTP服务那样丰富强大的命令指令功能，甚至不能提供目录的遍历浏览，在安全性方面也不能提供类似于FTP服务的验证功能，而且传输基于UDP协议的69端口号，导致文件的传输过程也并不像FTP协议那样可靠。但由于TFTP协议不需要客户端的权限验证，也不需要太多无必要的系统和网络带宽损耗，因此可以提供比FTP协议更有效率的文件传输体验。咱们可以安装一下TFTP简单文件传输服务的软件包来动手体验一番。

```bash
yum install tftp-server tftp
```

在红帽RHEL7系统中TFTP服务是基于xinetd服务程序来管理的，xinetd服务可以用于管理需要的Linux系统服务，并且还提供有了强大的日志功能。简单来说，当安装TFTP简单文件传输协议的软件包后，还需要在xinetd服务程序中开启一下，把默认的禁用(disable)参数修改为no。

```
vim /etc/xinetd.d/tftp
service tftp
{
        socket_type             = dgram
        protocol                = udp
        wait                    = yes
        user                    = root
        server                  = /usr/sbin/in.tftpd
        server_args             = -s /var/lib/tftpboot
        disable                 = no
        per_source              = 11
        cps                     = 100 2
        flags                   = IPv4
```

重启xinetd服务并添加到开机启动项中，有些系统的防火墙默认没有允许UDP协议的69端口，因此再手动的将其端口号加入到允许策略中吧。

```bash
#重启xinetd
systemctl restart xinetd
systemctl enable xinetd
#开启防火墙udp69端口
firewall-cmd --permanent --add-port=69/udp
success
firewall-cmd --reload
```

TFTP简单文件传输服务的根目录在/var/lib/tftpboot，因此可以用刚刚安装好的tftp命令来尝试获取一下文件，亲身体验一下TFTP带来的超简单文件传输过程。

tftp命令可用的参数

|命令|作用|
|:|
|?|帮助信息|
|put|上传文件|
|get|下载文件|
|verbose|显示详细的处理信息|
|status|显示当前的状态信息|
|binary|使用二进制进行传输|
|ascii|使用ASCII码进行传输|
|timeout|设置重传的超时时间|
|quit|退出|

### 使用Samba或NFS实现文件共享
#### SAMBA文件共享服务
在前面一个章节学习的FTP文件传输服务确确实实让主机之间传输文件变得非常方便，但FTP协议的本质是传输文件，并不是共享文件，要想让客户端能够直接在服务端上面修改文件内容还是比较麻烦的事情。于是在1987年时，由微软和英特尔公司共同制订了SMB服务器通信协议(Server Messages Block)，这项技术的诞生是为了解决局域网内的文件或打印机等资源的共享服务问题，让多个主机之间共享文件变成越来越简单。

后来到了1991年，当年还在读大学的学生Tridgwell为了解决Linux与Windows系统之间的文件共享问题，便基于了这项SMB技术协议开发出了SMBserver这一款服务程序，SMBserver服务程序是一款基于SMB协议并由服务端和客户端组成的开源文件共享软件，通过非常简单的配置就能够实现Linux系统与Windows系统之间的文件共享工作。当时还在上学的Tridgwell想要把这款SMBServer软件注册成为商标，但却被商标局以SMB是没有意义的字符而拒绝了他的申请，经过Tridgwell不断的翻看词典，突然看到一个拉丁舞蹈的名字——SAMBA，如图12-1所示，这个热情洋溢的舞蹈名字中又恰好包含了SMB(SAMBA)，于是这便是Samba服务程序名字的由来，现在已经成为了Linux系统与Windows系统之间共享文件的最佳选择。

```
yum install samba
```

由于这次配置文件中的注释信息行实在太多，不便于分析里面的重要参数，因此咱们可以先把配置文件改个名字，然后使用cat命令读入主配置文件内容后通过grep命令-v参数（反向选择）分别去掉所有以#（井号）和;（分号）开头的注释信息行，对于剩余的空白行可以再用"^$"来表示并反选过滤，最后把过滤后的可用参数信息通过重定向符覆盖写入到原始文件名称中即可。samba服务程序过滤后的参数并不复杂，为了更方便同学们查阅参数功能，刘遄老师在重要参数行后面都写上了注释说明：

```
mv /etc/samba/smb.conf /etc/samba/smb.conf.bak
cat /etc/samba/smb.conf.bak | grep -v "#" | grep -v ";" | grep -v "^$" > /etc/samba/smb.conf
cat /etc/samba/smb.conf
```

```

```
[global]		#全局参数。
workgroup = MYGROUP	#工作组名称。
server string = Samba Server Version %v	#服务器介绍信息,参数%v为显示SMB版本号。
log file = /var/log/samba/log.%m	#定义日志文件存放位置与名称，参数%m为来访的主机名。
max log size = 50	#定义日志文件最大容量为50Kb。
security = user	#安全验证的方式,总共有4种。
#share:来访主机无需验证口令，更加方便，但安全性很差。
#user:需由SMB服务验证来访主机提供的口令后才可建立访问,更加的安全。
#server:使用独立的远程主机验证来访主机提供的口令（集中管理帐号）。
#domain:使用PDC来完成验证
passdb backend = tdbsam	#定义用户后台的类型，共有3种。
#smbpasswd:使用SMB服务的smbpasswd命令给系统用户设置SMB密码。
#tdbsam:创建数据库文件并使用pdbedit建立SMB独立的用户。
#ldapsam:基于LDAP服务进行帐户验证。
load printers = yes	#设置是否当Samba服务启动时共享打印机设备。
cups options = raw	#打印机的选项
[homes]		#共享参数
comment = Home Directories	#描述信息
browseable = no	#指定共享是否在“网上邻居”中可见。
writable = yes	#定义是否可写入操作，与"read only"相反。
[printers]		#打印机共享参数
comment = All Printers	
path = /var/spool/samba	#共享文件的实际路径(重要)。
browseable = no	
guest ok = no	#是否所有人可见，等同于"public"参数。
writable = no	
printable = yes
```

##### 配置共享资源
Samba服务程序的配置文件与前面学习过的Apache服务很相似，包括有全局配置参数和局部配置参数。全局配置参数是Samba服务程序对整体共享环境的设置，是对里面每个独立的共享资源都生效的参数，而区域配置参数指的则是一个个独立可用的共享资源，共享资源的创建方法很简单，只需要按照下面参数格式写入配置文件后重启服务程序即可~

```
参数	作用
[linuxprobe]	共享名称为linuxprobe
comment = Do not arbitrarily modify the database file	警告用户不要随意修改数据库
path = /home/database	共享文件夹在/home/database
public = no	关闭所有人可见
writable = yes	允许写入操作
```

第1步：创建用于访问共享资源的帐户信息，在红帽RHEL7系统中Samba服务程序默认使用的是用户口令认证模式（user），可以做到仅让有密码、受信任的用户访问共享资源，验证的过程也十分的简单。不过咱们需要为Samba服务程序建立独立的用户密码数据库后才能登陆使用，另外Samba服务的数据库要求帐号必须是当前系统中已经存在的用户，否则日后创建文件将产生权限属性的混乱错误。

pdbedit命令用于管理SMB服务的帐户信息数据库，格式为：“pdbedit [选项] 帐户”，第一次把用户信息写入到数据库时需要使用-a参数，以后修改用户密码、删除用户等等操作就不再需要了：

|参数	作用|
|:|
|-a 用户名|建立Samba用户|
|-x 用户名|删除Samba用户|
|-L|列出用户列表|
|-Lv|列出用户详细信息的列表|

第2步：创建用于共享资源的文件目录，不光要考虑到文件访问和写入权限的问题，而且由于/home目录是系统中普通用户的家目录，因此还需要考虑到文件上面SELinux安全上下文的监管控制。实际上刚刚过滤掉的注释信息中就提醒有了相关的SELinux安全上下文策略的说明，咱们只需要按照给的值进行修改即可，最后再记得执行下restorecon命令让目录文件上面新设置的SELinux安全上下文立即生效下哦~

```
mkdir /home/database
chown -Rf linuxprobe:linuxprobe /home/database
semanage fcontext -a -t samba_share_t /home/database
restorecon -Rv /home/database
```

第3步：设置SELinux服务对Samba程序访问个人用户家目录的允许策略，过滤筛选出所有与samba服务程序相关的SELinux域策略，根据策略的名称和经验选择出合适的策略条目进行开启即可：

```
getsebool -a | grep samba
setsebool -P samba_enable_home_dirs on
```

4步：在Samba服务程序的主配置文件中根据前面所提到的格式写入共享信息，在原始的配置文件中[homes]参数为共享该用户的家目录数据，[printers]参数为共享打印机设备，这两项如果您今后的工作中不需要，可以手动的删除掉，没有问题的~

```
vim /etc/samba/smb.conf 
[global]
 workgroup = MYGROUP
 server string = Samba Server Version %v
 log file = /var/log/samba/log.%m
 max log size = 50
 security = user
 passdb backend = tdbsam
 load printers = yes
 cups options = raw
[database]
 comment = Do not arbitrarily modify the database file
 path = /home/database
 public = no
 writable = yes
 ```
 
第5步：把上述步骤完成后也就基本完成了Samba服务程序的配置工作了，Samba服务程序叫做smb，因此重启一下smb服务，清空下iptables防火墙就可以来检验配置效果了：

 ```
systemctl restart smb
systemctl enable smb
```

##### Windows挂载共享
##### Linux挂载共享
刚刚可能不小心让大家产生了一些小误解，认为Samba服务只是为了解决Linux系统和Windows系统的资源共享问题而设计的，其实Samba服务程序还可以实现Linux系统之间的文件共享哦，在客户端安装一下共享资源的支持软件包（cifs-utils）：

```
yum install cifs-utils
```

在Linux系统客户端上面依次按照Samba服务用户名、密码、共享域的顺序写入到一个认证文件中，并为了保证不被其他人随意看到，最后再把其权限修改为仅root超级用户才能够读写

```
vim auth.smb
username=linuxprobe
password=redhat
domain=MYGROUP

chmod -Rf 600 auth.smb
```

完成上述步骤后就可以在客户端上面创建一个用于挂载Samba服务共享资源的目录文件，并把挂载信息写入到/etc/fstab文件中，这样保证远程共享挂载信息在服务器重启后依然生效

```
mkdir /database
vim /etc/fstab
//192.168.10.10/database /database cifs credentials=/root/auth.smb 0 0
```

#### NFS网络文件系统
如果觉得Samba服务程序配置起来实在很麻烦，而且恰巧需要共享文件的主机都是Linux系统，那么刘遄老师非常推荐大家使用NFS服务来共享文件给客户端，NFS网络文件系统(Network Files System)是一个能够把Linux系统的远程文件共享资源挂载到本地目录的服务，NFS文件系统协议允许网络中的主机通过TCP/IP协议进行资源共享，能够让Linux客户端像使用本地资源一样读写远端NFS服务端的文件内容。另外由于NFS网络文件系统服务已经在红帽RHEL7系统中默认安装好，配置步骤也十分的简单，因此刘遄老师有时讲课会开玩笑说NFS是need for speed，接下来与大家分享下NFS服务配置起来的爽快体验~请大家先自行用yum软件仓库检查确认下NFS软件包是否已经安装好吧

```
yum -y install nfs-utils
```

###### 配置NFS服务端
在NFS服务端主机上面建立用于NFS文件共享的目录，设置较大的权限来保证其他人也一样有写入的权限

```
mkdir /nfsfile
chmod -Rf 777 /nfsfile
echo "welcome to linuxprobe.com" > /nfsfile/readme
```

NFS服务程序的配置文件为/etc/exports，默认里面是空白没有内容的，可以按照共享目录的路径 允许访问的NFS资源客户端（共享权限参数）的格式来写入参数，定义要共享的目录与相应的权限。例如想要把/nfsfile目录共享给所有属于192.168.10.0/24这个网段的用户主机，并且让这些用户拥有读写权限，自动同步内存数据到本地硬盘，以及把对方root超级用户映射为本地的匿名用户等等特殊权限参数，那么就可以按照下面的格式来写入配置文件：

|参数|作用|
|:|
|ro|只读默认|
|rw|读写模式|
|root_squash|当NFS客户端使用root用户访问时，映射为NFS服务端的匿名用户。|
|no_root_squash|当NFS客户端使用root用户访问时，映射为NFS服务端的root用户。|
|all_squash|不论NFS客户端使用任何帐户，均映射为NFS服务端的匿名用户。|
|sync|同时将数据写入到内存与硬盘中，保证不丢失数据。|
|async|优先将数据保存到内存，然后再写入硬盘，效率更高，但可能造成数据丢失。|

```
vim /etc/exports
/nfsfile 192.168.10.*(rw,sync,root_squash)
```

启动运行NFS共享服务程序，由于NFS服务在文件共享过程中是依赖RPC服务进行工作了，RPC服务用于把服务器地址和服务端口号等信息通知给客户端，因此要使用NFS共享服务的话，顺手也要把rpcbind服务程序启动，并且把这两个服务一起加入到开机启动项中吧

```
systemctl restart rpcbind
systemctl enable rpcbind
systemctl start nfs-server
systemctl enable nfs-server
```

###### 配置NFS客户端
配置NFS客户端的步骤也十分简单，首先用showmount命令查询NFS服务端的远程共享信息，输出格式为“共享的目录名称 允许使用客户端地址”：

|参数|作用|
|:|
|-e|显示NFS服务端的共享列表|
|-a|显示本机挂载NFS资源的情况|
|-v|显示版本号|

```
showmount -e 192.168.10.10
```

然后在客户端系统上面创建一个挂载目录，使用mount命令的-t参数指定挂载文件系统的类型，以及后面写上服务端的IP地址，共享出去的目录以及挂载到系统本地的目录。

```
mkdir /nfsfile
mount -t nfs 192.168.10.10:/nfsfile /nfsfile
```

最后挂载成功后就应该能够顺利查看到刚刚写入文件内容了，当然如果同学们希望远程NFS文件共享能一直有效，还可以写入到fstab文件中

```
vim /etc/fstab
192.168.10.10:/nfsfile /nfsfile nfs defaults 0 0
```

#### AutoFs自动挂载服务
不论咱们是想使用前面学习的Samba还是NFS服务，都要把挂载信息写入到/etc/fstab中，这样远程共享资源就会自动的随服务器开机而挂载，虽然这样一劳永逸确实很方便，但如果挂载的远程资源很多的话，毕竟会或多或少消耗着服务器的一定网络带宽和CPU计算性能。如果挂载后长期不去使用这些服务，那么无疑这笔损耗是白白被浪费掉了，当然还会有同学说可以每次使用前都执行下mount命令手动的挂载上，这虽然是个不错的选择，但每次都要去挂载一下再使用，这样是不是又很麻烦呢？

AutoFs服务程序与Mount命令不同之处在于它是一种守护进程，只有检测到用户试图访问一个尚未挂载的文件系统时才自动的检测并挂载该文件系统。换句话说，把挂载信息填入/etc/fstab文件后系统将在每次开机时都自动把其挂载，而运行AutoFs服务后则是当用户需要使用该文件系统了才会动态的挂载，节约服务器的网络与系统资源。

```
yum install -y autofs
```

生产环境中一般都会同时管理着很多设备的挂载工作，如果把这些设备挂载信息都写入到autofs服务主配置文件中的话肯定无疑会让主配置文件臃肿不堪，不利于服务执行效率，也不利于日后修改里面的配置内容，所以在autofs服务程序的主配置文件中需要按照“挂载目录 子配置文件”的格式写入参数。挂载目录是设备要挂载位置的上一级目录，例如咱们的光盘设备一般是挂载到/media/cdrom目录中的，那么此处就应该写成/media即可，而对应的子配置文件则是对这个目录内挂载设备信息的进一步说明，子配置文件是需要用户自行定义的，文件名字没有严格要求，但后缀需以.misc结束。

```
vim /etc/auto.master
/media /etc/iso.misc
```

子配置文件中应按照“挂载目录 挂载文件类型及权限 :设备名称”的格式写入参数，例如想要把设备挂载到/media/iso目录中，则此时写iso即可，而-fstype为文件系统格式参数，iso9660为光盘系统设备格式，ro、nosuid及nodev为光盘设备具体的权限参数，最终/dev/cdrom则是定义要挂载的设备名称，配置完成后顺手再把autofs服务程序启动并加入到开机启动项中吧。

```
vim /etc/iso.misc
iso   -fstype=iso9660,ro,nosuid,nodev :/dev/cdrom

systemctl start autofs 
systemctl enable autofs
```

## 第十四节课
### 使用BIND提供域名解析服务
#### DNS域名解析服务
域名相比于IP地址来讲更有寓意，因此也更容易被记住，所以用户通常更习惯输入域名来访问网络中的资源，但在互联网中计算机之间只能基于IP地址来识别对方主机，互联网中传输数据也必须是基于外网的IP地址才能完成。因此为了降低网民访问互联网网站等资源的门槛，就出现一项叫做DNS域名解析服务(Domain Name System)的技术，这是一项用于管理和解析域名与IP地址对应关系的服务，简单来说就是能够接受用户输入的域名或IP地址，然后自动查找出匹配的信息，使用这项技术后咱们只需要在浏览器中输入一串域名就能打开想要访问的网站了。

平时同学们使用最多的就是输入某个域名来访问网站了吧，这其实仅仅是DNS域名解析服务功能的一小部分而已。DNS技术能够提供正向解析和反向解析，正向解析就是根据用户输入的某个域名查找到对应的IP地址信息，而反向解析则是根据用户输入的IP地址查找到对应的域名信息。对于互联网庞大的域名和IP地址对应关系数据库，DNS服务协议采用类似目录树的层次结构记录域名与IP地址的映射对应关系，形成一个分布式的数据库系统。

域名后缀被分为了国际域名、国内域名和国外域名三类，原则上对每个后缀都是有定义的，但实际使用上可以不必严格遵守，目前使用最广泛的后缀就是.com，原意是商业公司（Commercial organizations），其次还有.org（非营利组织）、.gov（政府部门）、.net（网络服务商）、.edu（教研机构）、.pub（公众大众）、.cn（中国国家）等等十几个常见的域名后缀。

当今世界越来越信息化，大数据、云计算、物联网、人工智能等等新鲜技术也不断涌出，全球网民数量据说已经要超过了35亿，而且还在以每年10%的速度飞速增长。假设每个网民每天只访问一个网站，而且只访问一次，那么也会产生出35亿次左右的查询请求，如此庞大的请求数量肯定无法由某一个服务器全部处理掉。DNS技术作为互联网基础设施服务，为了能够为网民提供不间断、稳定、快速的域名查询服务，进而保证互联网的正常运转，按照工作形式上分为有主服务器、从服务器和缓存服务器三种类型：

>主服务器:在特定区域内具有唯一性、负责维护该区域内的域名与IP地址对应关系。
>从服务器:从主服务器中获得域名与IP地址对应关系并维护，以防主服务器宕机等情况。
>缓存服务器:通过向其他域名解析服务器查询获得域名与IP地址对应关系，提高重复查询时的效率。

简单来说，主服务器就是真正管理域名和IP地址关系的。从服务器是帮助主服务器打下手，在各个国家、省市、地区、街道中进行部署，让用户能够就近进行域名查询，进而减轻主服务器负载压力。而缓存服务器是一种不太常用的工作形式，一般是工作在企业内网的网关位置，加速用户域名查询请求。这三种DNS工作形式将会在下面逐一进行讲解，现在仅需要大致了解即可。

DNS域名解析服务采用分布式数据结构保存海量区域数据信息，递归查询是指用于客户机向DNS服务器的查询请求，而迭代查询是指用于DNS服务器向其它DNS服务器的查询请求。根据咱们刚刚学习过的DNS服务器工作形式以及技术服务方式，可以猜想下当用户向就近的一台DNS服务器发起了对某个域名的查询请求之后的事情，

```
1. 检查HOSTS文件
2. 检查主机本地缓存
3. DNS区域信息解析库
4. 检查服务器本地缓存
5. 向根域DNS发起查询
6. 向com.域DNS发起查询
7. 向linuxprobe域DNS发起查询
8. 向www域发起查询
```


当用户向就近的DNS服务器请求一个域名时，正常情况会由本地DNS服务器向指定的DNS服务器主机发送迭代查询请求，而如果该DNS服务器中也没有要查询的数据，则会进一步向上级DNS服务器进行多次迭代查询，其中最高级、最权威的根DNS服务器主机总共有13台，分布在世界各地：

|名称|管理单位|地理位置|IP地址|
|:|
|A|INTERNIC.NET|美国-弗吉尼亚州|198.41.0.4|
|B|美国信息科学研究所|美国-加利弗尼亚州|128.9.0.107|
|C|PSINet公司|美国-弗吉尼亚州|192.33.4.12|
|D|马里兰大学|美国-马里兰州|128.8.10.90|
|E|美国航空航天管理局|美国加利弗尼亚州|192.203.230.10|
|F|因特网软件联盟|美国加利弗尼亚州|192.5.5.241|
|G|美国国防部网络信息中心|美国弗吉尼亚州|192.112.36.4|
|H|美国陆军研究所|美国-马里兰州|128.63.2.53|
|I|Autonomica公司|瑞典-斯德哥尔摩|192.36.148.17|
|J|VeriSign公司|美国-弗吉尼亚州|192.58.128.30|
|K|RIPE NCC|英国-伦敦|193.0.14.129|
|L|IANA|美国-弗吉尼亚州|199.7.83.42|
|M|WIDE Project|日本-东京|202.12.27.33|

#### 安装Bind服务程序
BIND伯克利互联网域名服务(Berkeley Internet Name Daemon)是一款全球互联网使用最广泛、能够提供安全可靠、快捷高效的域名解析的服务程序。DNS域名解析服务作为互联网的基础设施服务，其责任之重同学们可想而知，因此刘遄老师推荐您在安装bind服务程序的时候加上chroot扩展包，chroot俗称为监牢安全机制，能够有效的限制bind服务程序仅能对自身配置文件进行操作，从而保证了整个服务器的安全，在生产环境中一直力荐使用的功能~

```
yum -y install bind-chroot
```

Bind服务程序的配置并不简单，因为要想为用户提供健全的DNS查询服务，首先就要在本地保存有相关的域名数据库，而如果把所有域名和IP地址对应关系都写入到某个配置文件中，那估计要有上千万条的参数了，这样既不利于程序的执行效率，也不方便今后修改维护数据。因此在bind服务程序中有三个比较关键的地方，首先区域配置文件（/etc/named.rfc1912.zones）是用来保存域名和IP地址对应关系所在位置的文件，也就是说像是咱们书籍的目录一样，对应着每个域和IP地址段具体所在的位置，当需要查看或修改的时候根据里面的位置找到相关文件即可。其次数据配置文件目录（/var/named）是用来保存域名和IP地址真实对应关系的目录，咱们把需要为用户解析的域、IP地址段关系文件放置在该目录中即可，最后与往常不同的是主配置文件（/etc/named.conf），bind服务程序的主配置文件只有58行，其中除去注释信息和空行后实际有效参数才仅仅30行左右，仅用于定义bind服务程序的运行参数而已。

BIND伯克利互联网域名服务的服务程序名称叫做named，首先需要在/etc目录中找到对应的主配置文件，然后把第11、17行的地址均修改为any，分别代表着服务器上所有IP地址均可提供DNS查询服务，以及允许所有人对本机进行DNS查询请求，这两个要修改的地方请同学们一定要留心修改好。

```
vim /etc/named.conf
listen-on port 53 { any; };
allow-query { any; };
```

如前面所讲到的，在bind服务程序中的区域配置文件（/etc/named.rfc1912.zones）是用来保存域名和IP地址对应关系所在位置的文件，切记这个文件用于定义域名与IP地址解析规则保存的文件位置以及区域服务类型等内容，而不是往里面写入具体的域名、IP地址对应关系等等信息。服务类型包括有三种，分别为hint(根区域)、master(主区域)、slave(辅助区域)，其中常用的master和slave就是主服务器和从服务器的意思。

接下来的实验中会分别对主配置文件、区域信息文件与数据文件做修改，如果实践操作中服务启动失败，您怀疑是参数写错导致的话，可以执行下named-checkconf命令或named-checkzone命令来分别用于检查主配置与区域数据文件中语法或参数的错误。

##### 正向解析实验
DNS域名解析服务的正向解析是根据主机名(域名)查找到对应的IP地址，这是平时最常用的工作模式，也就是说当用户输入了一个域名后bind服务程序会进行自动匹配，然后把查询到对应的IP地址返回给用户。

第1步：编辑区域配置文件，该文件内默认已经有了一些无关紧要的解析参数，主要目的是为了让用户有个参考而已，同学们可以把下面参数添加至区域配置文件内容的最下面即可，当然也可以把原有信息全部清空，只保留自己的域名解析信息

```
vim /etc/named.rfc1912.zones
zone "linuxprobe.com" IN {
type master;
file "linuxprobe.com.zone";
allow-update {none;};
};
```

第2步：编辑域名数据文件，咱们可以从/var/named目录中复制一份正向解析的模板文件（named.localhost），然后把域名和IP地址的对应数据填写到文件后保存即可，复制的时候请加上-a参数，意在保留原始文件的所有者、所有组、权限属性等信息，这样让bind服务程序能顺利的读取该文件的内容：

```
cd /var/named/
ls -al named.localhost
cp -a named.localhost linuxprobe.com.zone
```

然后编辑域名数据文件，记得编写后要重启一下named服务才能让新的解析数据生效哦~另外由于正向解析文件中的参数较多、而且相对都比较重要，所以刘遄老师在每个参数后面都做了简要说明

```
vim linuxprobe.com.zone
systemctl restart named
```

```
$TTL 1D	#生存周期为1天				
@	IN SOA	linuxprobe.com.	root.linuxprobe.com.	(	
#授权信息开始:	#DNS区域的地址	#域名管理员的邮箱(不要用@符号)	
0;serial	#更新序列号
1D;refresh	#更新时间
1H;retry	#重试延时
1W;expire	#失效时间
3H;)minimum	#无效解析记录的缓存时间
NS	ns.linuxprobe.com.	#域名服务器记录
ns	IN A	192.168.10.10	#地址记录(ns.linuxprobe.com.)
IN MX 10	mail.linuxprobe.com.	#邮箱交换记录
mail	IN A	192.168.10.10	#地址记录(mail.linuxprobe.com.)
www	IN A	192.168.10.10	#地址记录(www.linuxprobe.com.)
bbs	IN A	192.168.10.20	#地址记录(bbs.linuxprobe.com.)
```

nslookup命令用于检测能否从网络DNS服务器中查询到域名与IP地址的解析记录，进而更准确的检查DNS服务是否已经能够为用户提供服务。

```
nslookup www.linuxprobe.com
```

##### 反向解析实验
DNS域名解析服务的反向解析作用是根据用户提交的IP地址查找到对应的域名信息，这种解析方式相比于正向解析来讲确实很少被使用。一般的作用就是对某个IP地址上所有绑定的域名进行整体屏蔽，屏蔽由这些域名发送出来的垃圾邮件，也可以针对某个IP地址进行域名反向解析，能够大致判断出有多少个网站正在运行在上面，对于购买虚拟主机时判断有无严重超售问题很有参考价值。

第1步：编辑区域配置文件，除了格式不要写错以外，同学们还需要记住此处定义的数据文件名称，一会要再/var/named目录中建立与其同名对应的文件才算配置妥当，反向解析是把IP地址解析成域名格式，因此定义的zone（区域）应该要把IP地址反写，原先是192.168.10.0，那么反写后就是10.168.192啦，只写IP地址的网络位，把下列参数添加至正向解析参数后面吧：

```
vim /etc/named.rfc1912.zones
zone "10.168.192.in-addr.arpa" IN {
type master;
file "192.168.10.arpa";
};
```

第2步：编辑域名数据文件，首先在/var/named目录中复制一份反向解析的模板文件（named.loopback），然后把下面参数填写至文件中即可，IP地址仅需要写主机位。

```
cp -a named.loopback 192.168.10.arpa
vim 192.168.10.arpa
systemctl restart named
```

```
$TTL 1D				
@	IN SOA	linuxprobe.com.	root.linuxprobe.com.	(
0;serial
1D;refresh
1H;retry
1W;expire
3H);minimum
NS	ns.linuxprobe.com.	
ns	A	192.168.10.10	
10	PTR	ns.linuxprobe.com.	#PTR为指针记录，仅用于反向解析中。
10	PTR	mail.linuxprobe.com.	
10	PTR	www.linuxprobe.com.	
20	PTR	bbs.linuxprobe.com.
```

第3步：检验解析结果，刚刚正向解析实验中已经把系统网卡中DNS地址参数修改成了本机IP地址，因此可以直接使用nslookup命令来检验解析结果，仅需输入IP地址即可查询到对应的域名信息

```
nslookup 192.168.10.10
```

#### 部署从服务器
作为互联网的基础设施服务，咱们有必要保证DNS域名解析服务的正常运转，为网民提供不间断、稳定、快速的域名查询服务。从服务器是DNS工作模式的一种类型，部署从服务器主要就是为了减轻主服务器的负载压力，以及当网民访问就近的本地DNS服务器时还能提升查询效率，也就是说从服务器可以从主服务器上抓取指定的区域数据文件，起到备份解析记录与负载均衡的作用。

第1步：在主服务器的区域信息文件中允许该从服务器的更新请求，即修改allow-update { 允许更新区域信息的主机地址;};参数，然后记得重启下主服务器的DNS服务程序哦。

```
vim /etc/named.rfc1912.zones
zone "linuxprobe.com" IN {
type master;
file "linuxprobe.com.zone";
allow-update { 192.168.10.20; };
};
zone "10.168.192.in-addr.arpa" IN {
type master;
file "192.168.10.arpa";
allow-update { 192.168.10.20; };
};

systemctl restart named
```

第2步：在从服务器中填写主服务器地址与要抓取的区域信息，然后重启服务即可。注意此时的服务类型应该是slave（从），而不再是master（主），masters参数后面应该为主服务器的IP地址，以及file参数后面定义的是同步文件后要保存到的位置，稍后可以在该目录内看到同步的文件。

```
vim /etc/named.rfc1912.zones
zone "linuxprobe.com" IN {
type slave;
masters { 192.168.10.10; };
file "slaves/linuxprobe.com.zone";
};
zone "10.168.192.in-addr.arpa" IN {
type slave;
masters { 192.168.10.10; };
file "slaves/192.168.10.arpa";
};

systemctl restart named
```

第3步：检验解析结果，当重启从服务器的DNS服务程序后，一般就已经自动从主服务器上面同步了域名数据文件，默认会放置在区域配置文件中所定义的目录位置中。随后修改下从服务器的网卡参数，把DNS地址参数修改成192.168.10.20，这样即使用从服务器自身提供的DNS域名解析服务，最后就能用nslookup命令顺利的看到解析结果了。

```
cd /var/named/slaves
ls 
#192.168.10.arpa linuxprobe.com.zone
nslookup
```

#### 安全的加密传输
DNS域名解析服务是互联网的基础建设设施，几乎所有的网络应用都依赖于DNS解析服务做出的查询结果，如果互联网中的DNS解析服务不能正常运转，那么即使Web网站或Email邮局服务都运行正常，用户也无法找到并使用它们了。

13台全球根DNS服务器以及互联网中的DNS服务器绝大多数(超过95%)是基于BIND服务程序搭建的，BIND服务程序为了能够安全的提供解析服务，目前已经支持了TSIG(TSIGRFC 2845)加密机制，TSIG主要是利用密码编码方式保护区域信息的传送(Zone Transfer)，也就是说TSIG加密机制保证了DNS服务器之间传送域名区域信息的安全性。

第1步：在主服务器中生成密钥，dnssec-keygen命令用于生成安全的DNS服务密钥，常用参数如下表，命令参数格式为："dnssec-keygen [参数] "，生成一个主机名称为master-slave的128位HMAC-MD5算法的密钥文件，执行命令后默认会在当前目录中生成公钥和私钥文件，咱们需要把私钥文件中的Key参数后面的值记录下来，待会要写入到配置文件中。

参数	作用
-a	指定加密算法（包括:RSAMD5 (RSA)、RSASHA1、DSA、NSEC3RSASHA1、NSEC3DSA等）
-b	密钥长度(HMAC-MD5长度在1-512位之间)
-n	密钥的类型(HOST为与主机相关的)

```
dnssec-keygen -a HMAC-MD5 -b 128 -n HOST master-slave
```

```
cat Kmaster-slave.+157+46845.private

Private-key-format: v1.3
Algorithm: 157 (HMAC_MD5)
Key: 1XEEL3tG5DNLOw+1WHfE3Q==
Bits: AAA=
Created: 20170607080621
Publish: 20170607080621
Activate: 20170607080621
```

第2步：在主服务器中创建密钥验证文件，首先应进入到bind服务程序目录中，然后把刚刚生成出的密钥名称、加密算法和私钥加密字符串按照下面格式写入到文件中，最后为了安全起见，咱们把文件的所有组修改成named，权限设置的要小一点并把该文件在/etc目录中做一个硬链接文件：

```
cd /var/named/chroot/etc/

vim transfer.key
key "master-slave" {
algorithm hmac-md5;
secret "1XEEL3tG5DNLOw+1WHfE3Q==";
};

chown root:named transfer.key
chmod 640 transfer.key
ln transfer.key /etc/transfer.key
```

第3步：开启并加载bind服务的密钥验证功能，首先需要在主服务器的主配置文件中加载密钥验证文件，然后设置只对有master-slave密钥认证的DNS服务器允许同步域名区域数据文件：

```
vim /etc/named.conf
include "/etc/transfer.key";
options {
 listen-on port 53 { any; };
 listen-on-v6 port 53 { ::1; };
 directory "/var/named";
 dump-file "/var/named/data/cache_dump.db";
 statistics-file "/var/named/data/named_stats.txt";
 memstatistics-file "/var/named/data/named_mem_stats.txt";
 allow-query { any; };
 allow-transfer { key master-slave; };

 systemctl restart named
```

至此DNS主服务器的TSIG密钥加密传输功能就已经完成了，这样清空下DNS从服务器的区域数据同步目录，然后再次重启一下bind服务程序，但此时就已经不能像刚刚那样自动获取到域名区域数据文件啦。
>~不禁让刘遄老师想起来了在00后小学生贴吧里看到过的搞笑段子——“我喜欢上了班花，但她是QQ会员，总感觉自己配不上她”。

第4步：配置从服务器支持密钥验证，配置DNS从服务器和主服务器的方法其实大致相同，需要在bind服务程序的目录中创建密钥认证文件，设置相应权限后做链接文件到/etc/目录：

```
cd /var/named/chroot/etc

vim transfer.key
key "master-slave" {
algorithm hmac-md5;
secret "1XEEL3tG5DNLOw+1WHfE3Q==";
};

chown root:named transfer.key
chmod 640 transfer.key
ln transfer.key /etc/transfer.key
```

第5步：开启并加载从服务器的密钥验证功能，操作步骤也同样是在主配置文件中加载密钥认证文件，然后按照指定格式写上主服务器的IP地址和密钥名称，但注意参数位置不要太靠前，大约在43行左右比较合适，否则bind服务程序会因为没有加载完预设参数而报错：

```
vim /etc/named.conf
include "/etc/transfer.key";
dnssec-enable yes;
dnssec-validation yes;
dnssec-lookaside auto;
server 192.168.10.10
{
keys { master-slave; };
};
```

第6步：DNS从服务器同步域名区域数据，双方两台服务器的bind服务程序都已经配置妥当，并设置匹配到了相同的密钥文件，接下来在从服务器上面重启下bind服务程序，就又能顺利的同步到区域文件啦。

#### DNS缓存服务器
DNS缓存服务器(Caching DNS Server)是一种不负责域名数据维护、也不负责域名解析的DNS服务类型，简单来说缓存服务器就是把用户经常使用到的域名与IP地址解析记录保存在主机本地中，提升下次解析的效率。DNS缓存服务器一般用于对高品质上网有需求的企业内网之中，但实际的应用并不广泛，而且缓存服务器解析成功与否还与指定的上级DNS服务器允许策略相关，因此同学们当前仅需了解下即可。

在bind服务程序的主配置文件中添加缓存转发参数，在默认参数下方添加一行参数"forwarders { 上游DNS服务器地址; };"，上游DNS服务器地址指的是从何处取得区域数据文件，主要考虑查询速度、稳定性、安全性等因素，刘遄老师使用的是北京市公共DNS服务器:210.73.64.1，请读者选择前先Ping下能否通信，否则可能会导致解析失败!!

```
vim /etc/named.conf
options {
 listen-on port 53 { any; };
 listen-on-v6 port 53 { ::1; };
 directory "/var/named";
 dump-file "/var/named/data/cache_dump.db";
 statistics-file "/var/named/data/named_stats.txt";
 memstatistics-file "/var/named/data/named_mem_stats.txt";
 allow-query { any; };
 forwarders { 210.73.64.1; };

systemctl restart named
```

重启DNS服务后验证成果，把客户端主机网卡的DNS地址参数指向为DNS缓存服务器（192.168.10.10），如图13-8所示。这样即可让客户端使用由本地DNS服务器提供的域名查询解析服务，验证DNS缓存服务器是否可以正常解析域名。

#### 分离解析技术
现在喜欢看《Linux就该这么学》书籍的海外留学生也越来越多，如果继续把网站服务器（http://www.linuxprobe.com/）架设在北京市机房内，那么国外读者访问起来速度必然会很慢，若把网站服务器架设在美国机房的话，那又会让国内读者访问变得很麻烦。既然读者有需求，刘遄老师也不差钱，于是便可以购买多台服务器部署在全球各地，然后采用DNS服务的分离解析功能，即可让不同来源的读者即便访问的是相同的网址，最终获取网站数据的服务器也是不同的。简单来说，就像如图13-9所示，让国内用户自动匹配到北京服务器，而海外留学生自动匹配到美国服务器。

>DNS：北京：122.71.115.10
>DNS：美国：106.185.25.10

>北京用户：122.71.115.1
>海外用户：106.185.25.1

那么为了解决《Linux就该这么学》访问速度的问题，站务管理员已经在美国机房购买并架设好了书籍网站服务器，咱们需要动手部署DNS服务器并实现分离解析功能，让北京用户与美国用户访问相同域名时也能解析出不同的IP地址。建议同学们还原虚拟机到最初始状态后重启安装bind服务程序，避免多个实验之间产生冲突。

第1步：修改bind服务程序的主配置文件，把监听端口与允许查询主机修改为any，由于自己配置的DNS分离解析功能与DNS根域服务器配置参数有冲突，所以需要把51-54行左右的根域信息删除掉。

```
vim /etc/named.conf
删除下面内容
zone "." IN {
type hint;
file "named.ca";
};
```

第2步：编辑域名区域配置文件，把区域配置文件中原有默认的数据清空，然后按照以下格式写入参数。首先用acl参数分别定义了两个变量名称（china与american），这样当下面再需要匹配IP地址的时候，就只需输入变量名称即可，这样不仅更容易阅读识别，而且也利于今后修改维护。主要难点是理解view参数框内的作用，通过匹配用户到底来源于中国还是美国的IP地址，然后去分别加载不同的区域数据文件（linuxprobe.com.china与linuxprobe.com.american），这样当把对应的IP地址分别写入到区域数据文件后，即可实现DNS的分离解析功能，也就是说比如中国的用户访问linuxprobe.com域的时候，会去按照linuxprobe.com.china区域数据文件内的IP地址找到对应的服务器。

```
vim /etc/named.rfc1912.zones
acl "china" { 122.71.115.0/24; };
acl "american" { 106.185.25.0/24;};
view "china"{
match-clients { "china"; };
zone "linuxprobe.com" {
type master;
file "linuxprobe.com.china";
};
};
view "american" {
match-clients { "american"; };
zone "linuxprobe.com" {
type master;
file "linuxprobe.com.american";
};
};
```

第3步：建立区域数据文件，分别通过模板文件创建出两份不同名称的区域数据文件，名称应与上面区域配置文件中的参数对应：

```
cd /var/named
cp -a named.localhost linuxprobe.com.china
cp -a named.localhost linuxprobe.com.american
```

vim linuxprobe.com.china

```
$TTL 1D	#生存周期为1天				
@	IN SOA	linuxprobe.com.	root.linuxprobe.com.	(	
#授权信息开始:	#DNS区域的地址	#域名管理员的邮箱(不要用@符号)	
0;serial	#更新序列号
1D;refresh	#更新时间
1H;retry	#重试延时
1W;expire	#失效时间
3H;minimum	#无效解析记录的缓存时间
NS	ns.linuxprobe.com.	#域名服务器记录
ns	IN A	122.71.115.10	#地址记录(ns.linuxprobe.com.)
www	IN A	122.71.115.15	#地址记录(www.linuxprobe.com.)
```

vim linuxprobe.com.american

```
$TTL 1D	#生存周期为1天				
@	IN SOA	linuxprobe.com.	root.linuxprobe.com.	(	
#授权信息开始:	#DNS区域的地址	#域名管理员的邮箱(不要用@符号)	
0;serial	#更新序列号
1D;refresh	#更新时间
1H;retry	#重试延时
1W;expire	#失效时间
3H;minimum	#无效解析记录的缓存时间
NS	ns.linuxprobe.com.	#域名服务器记录
ns	IN A	106.185.25.10	#地址记录(ns.linuxprobe.com.)
www	IN A	106.185.25.15	#地址记录(www.linuxprobe.com.)
```

第4步：重新启动named服务程序后验证结果，设置客户端主机（Windows系统或Linux系统均可）的IP网卡地址分别为122.71.115.1与106.185.25.1，DNS地址分别为服务器主机的两个IP地址，这样当咱们尝试用nslookup命令解析域名的时候就会很清晰的看到解析结果，让不同来源的用户访问到不同的服务器。

```
nslookup www.linuxprobe.com
```

## 第十五节课
### 使用DHCP动态管理主机地址
#### 动态主机地址管理协议
DHCP动态主机地址管理协议(Dynamic Host Configuration Protocol)是一种基于UDP协议且仅限用于局域网内使用的网络协议，最主要的用途是为局域网内部设备或网络供应商自动分配IP地址等网卡参数，通常会应用在大型的局域网环境中或局域网内存在比较多的移动办公设备。DHCP服务程序能够使局域网内的主机自动且动态的获取IP地址、子网掩码、网关地址以及DNS服务器地址等网卡信息，因此能够有效的提升IP地址使用率，提高网络配置效率，减少管理和维护成本。

简单来说，DHCP协议就是让客户端自动获得网卡参数的服务，如图14-1所示，设想某机房内有多台主机，如果要给每台主机手工设置网卡参数一定非常麻烦，而且今后维护也要靠手动修改网卡参数才行，那如果机房内有100台或者1000台主机呢？手工设置网卡信息就变得十分笨拙，也没有那个运维人员原意每天做这么枯燥、重复性高的工作吧。DHCP协议不光能实现对网卡参数的自动分配，而且还能保证任何IP地址在同一时刻只能由一台客户机使用，且能够为指定主机分配固定的IP地址。

DHCP动态主机地址管理协议的应用十分广泛，不论是服务器机房还是家庭都会在日常中使用到它，比如某位同学开了一家咖啡馆，肯定很多顾客都想一边惬意的喝着咖啡一边连着WIFI刷会微信朋友圈吧，试想一下作为老板的您，需要在每位顾客设备上手动的设置下网卡IP地址、子网掩码、网关地址等等信息，这不用多想就知道不切实际了。另外还面临着一个潜在问题，因为每个类似于192.168.10.0/24的IP地址段最多只能容纳两百多个主机，而咖啡厅一天的客流量一定不止两百多人吧，如果给每个人都手工分配了一个IP地址，那么当顾客离开咖啡馆的时候也将带走这个IP地址，以后很难再交给其他人继续使用，造成了不小的资源浪费，也增加了IP地址的管理成本。

既然确定今后生产环境中肯定离不开DHCP动态主机地址管理协议了，那么也就很有必要好好熟悉一下DHCP的常见术语了：

>作用域:一个完整的IP地址段，DHCP协议根据作用域来管理网络的分布、分配IP地址及其他配置参数。
>超级作用域:用于同一物理网络上多个逻辑IP地址子网段，包含作用域的列表，并对子作用域统一管理。
>排除范围:把某些IP地址在作用域中排除，确保这些IP地址不会被提供给DHCP客户机。
>地址池:在定义DHCP服务的作用域并应用排除范围后，剩余用来动态分配给DHCP客户机的IP地址范围。
>租约:即DHCP客户机能够使用动态分配到的IP地址的时间。
>预约:保证局域子网中特定设备总是获取到相同的IP地址。

#### 部署dhcpd服务程序
dhcpd服务程序是Linux系统中用于提供DHCP动态主机地址管理协议的服务，确认yum仓库配置妥当后就可以直接安装了，DHCP动态主机地址管理服务功能虽然十分丰富强大，但dhcpd服务程序的配置步骤却十分简单，很大程度上降低了Linux系统实现DHCP动态主机地址管理服务的门槛。

```
yum -y install dhcp
```

是的，您没有看错！打开dhcpd服务程序的主配置文件发现只有3行注释语句，大意是告诉咱们配置文件需要全部由自己来写，如果不熟悉的话可以看下第2行中的参考示例文件，再或者买本刘遄老师的《Linux就该这么学》自学书籍吧~哈哈，最后一句是玩笑了。如图14-2所示，一个标准的DHCP配置文件应该包括全局配置参数、子网网段声明、地址配置选项以及地址配置参数：

```
cat /etc/dhcp/dhcpd.conf
ddns-update-style interim;
ignore client-updates;
subnet 192.168.10.1 netmask 255.255.255.0 {
	option routers 192.168.10.1;
	option subnet-mask 255.255.255.0;
	default-lease-time 21600;
	max-lease-time 43200;
}
```

全局配置参数用于定义DHCP服务的整体运行参数，而子网网段声明用于配置整个子网段的地址属性，dhcpd服务程序配置文件的参数比较多，刘遄老师为同学们挑选了最常用参数。并逐一进行了简单介绍，为接下来实验打下基础：

|参数|作用|
|:|
|ddns-update-style 类型|定义DDNS服务动态更新的类型，类型包括:none（不支持动态更新），interim（互动更新模式）与ad-hoc(特殊更新模式)。|
|allow/ignore client-updates|允许/忽略客户机更新DNS记录。|
|default-lease-time 21600|默认超时时间。|
|max-lease-time 43200|最大超时时间。|
|option domain-name-servers 8.8.8.8|定义DNS服务器地址。|
|option domain-name "domain.org"|定义DNS域名。|
|range|定义用于分配的IP地址池。|
|option subnet-mask|定义客户机的子网掩码。|
|option routers|定义客户机的网关地址。|
|broadcase-address|广播地址	定义客户机的广播地址。|
|ntp-server IP地址|定义客户机的网络时间服务器（NTP）。|
|nis-servers IP地址|定义客户机的NIS域服务器的地址。|
|hardware 硬件类型 MAC地址|指定网卡接口的类型与MAC地址。|
|server-name 主机名|通知DHCP客户机服务器的主机名。|
|fixed-address IP地址|将某个固定IP地址分配给指定主机。|
|time-offset 偏移差|指定客户机与格林尼治时间的偏移差。|

#### 自动管理IP地址
DHCP动态主机地址管理协议的设计初衷是为了更高效的集中管理局域网内IP地址资源，作为运维人员只需要在机房内部署一台DHCP服务器，接下来的事情就不用操心了。DHCP服务会自动把IP地址、子网掩码、网关、DNS地址等等网卡信息分配给有需要的客户端，当客户端的租约时间到期后还可以自动回收所分配的IP地址，以便交给后面新加入的客户主机。为了让实验更有挑战性，刘遄老师来模拟一个真实生产环境的需求吧——“机房运营部门: 明日约有50名外来学员自带笔记本设备来我司培训学习，请保证学员能够用机房本地DHCP服务器自动获取IP地址并正常上网”，机房网段及参数如下表所示：

|参数名称	值|
|:|
|默认租约时间	|21600秒|
|最大租约时间	|43200秒|
|IP地址范围|192.168.10.50~192.168.10.150|
|子网掩码|255.255.255.0|
|网关地址|192.168.10.1|
|DNS服务地址|192.168.10.1|
|搜索域|linuxprobe.com|

vim /etc/dhcp/dhcpd.conf

```
ddns-update-style none;	#设置DDNS服务不自动动态更新。
ignore client-updates;	#忽略客户机更新DNS记录。
subnet 192.168.10.0 netmask 255.255.255.0 {	#作用域为192.168.10.0/24网段。
range 192.168.10.50 192.168.10.150;	#IP地址池为192.168.10.50-150 #（约100个IP地址）。
option subnet-mask 255.255.255.0;	#定义客户机默认的子网掩码。
option routers 192.168.10.1;	#定义客户机的网关地址。
option domain-name "linuxprobe.com";	#定义默认的搜索域。
option domain-name-servers 192.168.10.1;	#定义客户机的DNS地址。
default-lease-time 21600;	 #定义默认租约时间。
max-lease-time 43200;	#定义最大预约时间。
}	#此为结束符
```

在红帽认证及生产环境中大多都需要把配置过的服务加入到开机启动项中。

```
systemctl start dhcpd
systemctl enable dhcpd
```

#### 分配固定IP地址
在DHCP动态主机地址管理协议中有个术语叫做“预约”，预约指的是保证局域网中特定的设备总是获取到固定的IP地址，换句话说就是dhcpd服务程序会把某个IP地址私藏下来，只有匹配到特定主机了才会拿出来分配，让某个特定主机总能获取到固定的IP地址。

要想把IP地址与某个主机相互绑定，那么需要该主机的MAC网卡物理地址才可以，MAC地址是网卡上面的一串独立标识符，因此不用担心冲突的情况，如图14-6所示，咱们在Linux系统或Windows系统中都可以通过查看网卡状态来查看到这个MAC值，dhcpd服务程序的配置参数格式如下：

```
host 主机名称 {				
hardware	ethernet	#该主机的MAC地址;	
fixed-address	#欲指定的IP地址;		
}
```

此时一定有同学会问如果不方便查看MAC地址怎么办呢？比如给公司老板的机器绑定IP地址，总不能随便就去看别人电脑信息吧，刘遄老师就来告诉大家一个很好的办法，首先咱们启动dhcpd服务程序，然后为老板的主机分配一个IP地址，这样在DHCP服务器本地的日志文件中就会保存有了这次的IP地址分配记录，通过看日志文件来获取到对方电脑的网卡MAC地址啦~

```
tail -f /var/log/messages
```

Windows系统中直接查看到的MAC地址是类似于00-0c-29-27-c6-12这样的，很明显MAC地址虽然值是一样的，但间隔符变成了-（减号），因此咱们在Linux系统中配置dhcpd服务程序的时候一定要保证里面的MAC地址都是以:（冒号）来间隔的哦~

```
vim /etc/dhcp/dhcpd.conf 
ddns-update-style none;
ignore client-updates;
subnet 192.168.10.0 netmask 255.255.255.0 {
range 192.168.10.50 192.168.10.150;
option subnet-mask 255.255.255.0;
option routers 192.168.10.1;
option domain-name "linuxprobe.com";
option domain-name-servers 192.168.10.1;
default-lease-time 21600;
max-lease-time 43200;
host linuxprobe {
hardware ethernet 00:0c:29:27:c6:12;
fixed-address 192.168.10.88;
}
}

systemctl restart dhcpd
```

### 使用Postfix与Dovecot部署邮件系统
#### 电子邮件系统
20世纪60年代正处在美苏两国的冷战时期，美国军方认为应该在科技技术上持续占据领先地位，这样有助于在未来的战争中取得优势，因此便由美国国防部资助并发起了一项叫做ARPANET的科研项目，这项科研项目即是大家所熟知的阿帕网计划，也就是当今互联网技术的雏形，阿帕网计划实现了人类首次意义上的封包交换网络。但很快在1971年就遇到了严峻问题，参于阿帕网科研项目的科学家工作在美国不同地区，甚至还因为时差的影响而不能及时的分享各自的研究成果，因此科学家们迫切的需要一种能够借助于网络且建立在计算机之间的传输数据的方法。

之前学习的WEB网站服务或FTP文件传输服务也能够实现数据交换，但这些传输方式都要像打电话一样，双方都必须同时在线才能完成传输工作，如果对方的主机宕机或科研人员临时离开，就有可能错过某些科研结果了。好在当时麻省理工学院的Ray Tomlinson博士也参与到了阿帕网计划的科研项目中，他觉得有必要设计一种类似于“信件”的传输服务，准备一个信箱，这样即便对方临时不在线也能够完成数据的接收，当对方上线后再来处理就可以了，于是Ray Tomlinson博士用了将近一年的时间就完成了Email电子邮件的设计，并在1971年秋天使用SNDMSG软件向自己的另一台电脑发送出了人类历史上第一封Email电子邮件，它标志着邮件系统在人类互联网中诞生了。

既然要在互联网中给其他人发送电子邮件，那么对方用户的昵称代号必须是具有唯一性的，否则这封邮件可能会同时发给多个重名的用户，也或者干脆谁都收不到了。因此当时Ray Tomlinson博士决定选择用“姓名@电脑主机名称”的格式来规范电子邮箱的名称，而选择用@符号做间隔符的原因其实也很简单，因为Ray Tomlinson博士觉得人类的姓名和电脑主机名称中应该不会有这么一个@符号吧~所以具有了相对的唯一性，就选择了这个符号！~

电子邮件系统的传输基于邮件协议完成，常见的邮件协议包括SMTP简单邮件传输协议，用于发送和中转发出的电子邮件，占用服务器的25/TCP端口号。POP3第三版邮局协议，用于把邮件存储到本地主机，占用服务器的110/TCP端口号。IMAP4第四版互联网信息访问协议，用于在本地主机上访问邮件，占用服务器的143/TCP端口号。

在电子邮件系统服务中用于为用户收发邮件的服务器被叫做MUA用户代理（Mail User Agent），另外既然电子邮件系统能够让客户不在线的情况下依然可以完成数据传输，那肯定要有一个用于帮助用户临时保存邮件数据的“信箱”服务器吧，这个用于保存用户邮件的服务器叫做MDA邮件投递代理（Mail Delivery Agent），它的工作主要是把来自于MTA的邮件保存到本机的收件箱中，而不同的电子邮件服务供应商之间发送邮件还要经过MTA邮件传输代理(Mail Transfer Agent)的转发处理，它的工作就是把来自于MUA的邮件转发至合适的MTA服务器中，例如从新浪邮箱发送一封邮件到谷歌邮件。

MUA->MTA->MTA->MUA

发信人(MUA)使用STMP发送邮件到新浪服务器(MTA)，新浪服务器(MTA)使用STMP发送邮件到谷歌服务器(MTA),谷歌服务器(MTA)使用POP3或IMAP4让收信人(MUA)接收邮件

总结来说，一般的网络服务程序传输信息就像拨打电话一样，需要对方当前也保持在线，否则会报错连接超时，而电子邮件系统的用户在发送邮件后就不必须等待投递工作完成再下线，因为如果对方主机宕机或临时离开了，那么发件服务器就会把要发送的内容自动的暂时保存到本地，检测到对方服务器恢复后会立即再次投递，这期间一般无需运维人员操作。另外如果同学们有兴趣学完之后在生产环境中部署一个企业级的电子邮件系统，刘遄老师给大家总结了四点需要注意的事项，首先是反垃圾与反病毒模块，它能够很有效的阻止垃圾邮件或病毒邮件对企业邮箱的干扰，其次是邮件加密，有效保护企业内邮件内容不被骇客盗取和篡改，而邮件监控审核模块则是很好的监管措施，有效的监控全体职员邮件内容中有无敏感词，透露企业资料等违规行为，最后就是要强调下稳定性了，电子邮件系统的稳定性至关重要，运维人员应做到保证电子邮件系统的稳定运行，并及时做好防范DDOS分布式拒绝服务攻击的准备。

#### 部署基础电子邮件系统
一个最基础的电子邮件系统肯定要包括有发件服务和收件服务，因此需要使用基于SMTP协议的Postfix服务程序来提供发件服务功能，以及用基于POP3协议的Dovecot服务程序来提供收件服务功能，这样客户端在使用类似于OutLook Express或Foxmail的客户端服务程序时就可以正常的收发信件了。

在红帽RHEL5、红帽RHEL6及诸多早期Linux系统中默认使用的发件服务是由sendmail服务程序来提供的，而在红帽RHEL7系统中已经替换成了Postfix服务程序，Postfix相比Sendmail给我最大的感觉就是配置变得简单了，减少了很多不必要的配置步骤，而且在稳定性、并发量等方面确实也有很大的改进，刘遄老师相信Postfix服务程序会一直保留下去的，同学们要好好学习啦~

人们生活中的邮箱地址一般都是类似于“root@linuxprobe.com” 这样的样子，也就是按照“用户名@主机地址（域名）”格式规范的，如果给我一串“root@192.168.10.10”的信息，刘遄老师可能猜不到是个邮箱地址，觉得更像是SSH协议的连接信息吧，因此要想更好的检验配置电子邮件系统的效果，需要先把bind服务程序部署起来，为服务器和客户端提供DNS域名解析的服务。

第1步：配置服务器主机名称，需要保证服务器主机名称与发信域名保持一致。

```
vi /etc/hostname
mail.linuxprobe.com
```

第2步：为电子邮件系统提供域名解析。

```
cat /etc/named.conf
10 options {
 11 listen-on port 53 { any; };
 12 listen-on-v6 port 53 { ::1; };
 13 directory "/var/named";
 14 dump-file "/var/named/data/cache_dump.db";
 15 statistics-file "/var/named/data/named_stats.txt";
 16 memstatistics-file "/var/named/data/named_mem_stats.txt";
 17 allow-query { any; };
```

```
cat /etc/named.rfc1912.zones
zone "linuxprobe.com" IN {
type master;
file "linuxprobe.com.zone";
allow-update {none;};
};
```

```
cat /var/named/linuxprobe.com.zone
$TTL 1D				
@	IN SOA	linuxprobe.com.	root.linuxprobe.com.	(
0;serial
1D;refresh
1H;retry
1W;expire
3H;minimum
NS	ns.linuxprobe.com.	
ns	IN A	192.168.10.10	
@	IN MX 10	mail.linuxprobe.com.	
mail	IN A	192.168.10.10
```

#### 配置Postfix服务程序
Postfix是一款由IBM集团出资研发的免费开源电子邮件服务程序，能够很好的兼容Sendmail服务程序，也就是说Sendmail用户可以很方便的迁移到新的服务上面，Postfix服务的收件、发件性能确实强过Sendmail服务，并且能够自动增加、减少进程的数量来保证电子邮件系统的高性能与稳定性，另外Postfix服务程序是由诸多的小模块组成，每个小模块都可以完成特定的功能，因此在今后的生产工作环境中可以灵活搭配它们。

第1步：安装Postfix服务程序，当然这一步在红帽RHEL7系统中是多余的，刘遄老师把步骤写下来的目的是为了让同学们读完这本《Linux就该这么学》后不仅能够掌握红帽RHEL系统，还能立即上手fedora、centos等等主流的Linux系统，当那些系统没有Postfix服务程序的时候也不用慌~对了，最后记得把iptables防火墙给禁用掉，否则会导致外部用户访问不了咱们的电子邮件系统。

```
yum -y install postfix
```

第2步：配置Postfix服务程序，对于初次看到Postfix服务程序主配置文件（/etc/postfix/main.cf）的同学一定会被吓到吧，竟然足足有679行左右呢，但其实不必担心，这里面绝大多数的内容依然是注释信息，而且刘遄老师一直在强调Linux系统正确的学习方法，负责任的好老师不应只是做书本的搬运工，更应该做一名优质内容的提炼者，因此刘遄老师翻遍了配置参数介绍，以及结合运维工作经验最终总结出了最应该掌握的7个参数：

|参数|作用|
|:|
|myhostname|邮局系统的主机名。|
|mydomain|邮局系统的域名。|
|myorigin|从本机寄出邮件的域名名称。|
|inet_interfaces|监听的网卡接口。|
|mydestination|可接收邮件的主机名或域名。|
|mynetworks|设置可转发那些主机的邮件。|
|relay_domains|设置可转发那些网域的邮件|

Postfix服务程序总共需要修改五处，首先是在约76行左右定义有一个名称为myhostname的变量，它用来保存服务器的主机名称，读者请记住这个变量的名称，下边参数需要调用它。

```
vim /etc/postfix/main.cf
myhostname = mail.linuxprobe.com
mydomain = linuxprobe.com
myorigin = $mydomain
inet_interfaces = all
mydestination = $myhostname, $mydomain
```

第3步:创建电子邮件系统的登陆帐号，Postfix与Vsftpd服务程序一样都可以调用本地系统帐号和密码，因此在本地进行常规帐号创建即可，最后再把配置妥当的postfix服务重启，加入到开机启动项中就大功告成了！~

```
useradd boss
echo "linuxprobe" | passwd --stdin boss

systemctl restart postfix
systemctl enable postfix
```

#### 配置Dovecot服务程序
Dovecot是一款能够为Linux系统提供IMAP和POP3电子邮件服务的开源软件程序，拥有极高的安全性，并且配置起来也十分简单，执行效率很快，而且占用的服务器硬件资源也较少，是非常推荐的电子邮件系统的收件服务软件。

第1步：安装Dovecot服务程序软件包

```
yum install dovecot
```

第2步：配置部署Dovecot服务程序，对Dovecot服务程序的主配置文件需要修改2-3处，首先是在主配置文件中的约24行左右，把Dovecot服务程序支持的电子邮件协议修改为imap、pop3和lmtp。然后在该行的下面添加一行参数来允许客户使用明文进行密码验证，这是由于Dovecot服务程序为了保证电子邮件系统安全而默认强制客户必须使用加密方式进行登陆，而当前由于咱们没有加密系统的支持，因此需要添加参数来允许客户的明文登陆行为。

```
vim /etc/dovecot/dovecot.conf
# Protocols we want to be serving.
protocols = imap pop3 lmtp
disable_plaintext_auth = no
```

最后是在主配置文件中的约48行左右，设置允许登陆的网段地址，也就是说同学们可以在这里限制只有来自于某个网段的客户才能使用电子邮件系统，如果想允许所有人都能来使用，可以不用修改本条参数：

```
login_trusted_networks = 192.168.10.0/24
```

第3步：配置邮件格式与存储路径，需要编辑dovecot服务程序单独的子配置文件，定义要把收到的邮件信息保存到服务器本地的路径，而这个路径默认已经是被定义好的，只需要把此配置文件中第25行前面的#（井号）注释信息去掉即可：

```
mail_location = mbox:~/mail:INBOX=/var/mail/%u
```

然后切换到该用户身份后在家目录中建立用于保存邮件的目录，记得重启一下服务并加入到开机启动项就完成了对Dovecot服务程序的全部配置部署步骤：

```
su - boss
mkdir -p mail/.imap/INBOX
exit

systemctl restart dovecot 
systemctl enable dovecot
```

#### 客户使用电子邮件系统
如何得知电子邮件系统已经能够正常收发信件了呢？咱们可以用目前最常用的Windows7系统来进行验证，刘遄老师选择用Windows系统自带的Outlook软件进行收发信件测试，当然或许很多同学都已经在工作中习惯Foxmail之类的软件来管理邮件，这都是没影响的，不论是什么邮件管理软件都能登陆到电子邮件系统中来。另外请同学们自行配置Windows7系统网卡参数以及DNS地址为服务器地址（192.168.10.10），这样才能正常的解析邮件域名。

当同学们使用Outlook软件成功发送邮件后便可以在电子邮件服务器上面使用mail命令查看到新邮件提醒了，而如果想查看邮件的完整内容只需输入前面的编号即可。

```
mail
```

#### 设置用户别名邮箱
用户别名功能是一项简便好用的邮件帐号伪装技术，可以设置多个虚拟邮箱帐号来接受他人邮件，以保证真实邮件地址不被泄露，同时还可以用于接收多个邮箱的信件内容。

```
cat /etc/aliases
newaliases
```

## 第十六节课
### 使用Squid部署代理缓存服务
#### 代理缓存服务
Squid一款在Linux系统中最为流行的高性能代理服务软件，通常它会被用作是Web网站的前置缓存服务，能够替代用户向网站源服务器请求页面数据并进行缓存。简单来讲，Squid服务程序会按照接收到的用户请求来获取网页、图片等用户所需的数据并自动存储在服务器内，当后面的用户再来请求相同数据时，则可以直接把刚刚储存在服务器本地的数据交给用户，这样不仅大幅减少了用户的等待时间，进而还能减轻网站源服务器的负载压力。

Squid服务程序配置起来相对简单，效率高，功能丰富，它能支持如HTTP、FTP、SSL等多种协议的数据缓存，基于ACL访问控制列表和ARL访问权限列表功能的内容过滤与权限管理功能，基于多种条件来禁止用户访问存在威胁或不适宜的网站资源，在保证企业内网安全的同时还整体的提高了客户机的网页访问速度，帮助节省网络带宽。由于缓存代理服务不仅会消耗服务器较多的CPU计算性能、内存及硬盘空间等硬件资源，同时还需要较大的网络带宽来保证给用户传送数据的效率，每年的网络带宽开销就有可能超过数十万元，因此很多IDC或CDN服务供应商就会把缓存代理节点服务器放置在国内二三线城市来节约成本，Squid服务程序在为用户提供缓存代理服务时，可以从功能作用上分为正向代理和反向代理：

正向代理模式是让用户通过Squid服务程序获取到网站页面等资源，具体服务方式上又可以分为标准代理模式与透明代理模式，标准正向代理模式是把网站数据缓存到服务器本地，提高数据资源被再次访问时的效率，但用户必需在上网时在浏览器等软件中填写代理服务器的IP地址与端口号信息，否则默认不使用代理服务，透明正向代理模式的功能作用与标准正向代理模式完全相同，区别是用户不需要手动指定代理服务器的IP地址与端口号，所以这种代理服务对于用户来讲是相对透明的工作模式。

正向代理 Client->Squid服务器->Internet

日常中网站普遍的加载有大量图片、视频等静态资源内存，这些静态资源相对来说是比较稳定的数据信息，咱们可以使用Squid服务程序提供的反向代理模式来响应用户访问网站页面中静态资源的请求。而且如果反向代理服务器中恰巧已经有了用户要访问的静态资源则直接把缓存的内容发送给用户，这样不仅加快了用户的网站访问速度，而且还在一定程度上降低了网站服务器的负载压力。


反向代理 Client->Squid服务器->Internet
Client、Squid服务器在同一个局域网

总结来说，正向代理模式一般会被用于企业局域网之中，让内网用户统一的通过Squid服务器访问互联网资源，不仅能够在一定程度上减少公网带宽开销，最主要的是还能做到网站内容的筛查限制，一旦内网用户访问的网站内容匹配到了禁止规则就会自动屏蔽网站，是非常不错的内容监管方案。而反向代理模式一般是为大中型网站提供缓存服务的，把网站中的静态资源保存在国内多个节点机房中，当有用户发起网站资源请求的时候可以就近为用户分配节点并传输资源，是一种在大型网站中十分普遍使用的技术。

#### 配置Squid服务程序
安装squid服务程序

```
yum install squid
```

Squid服务程序的配置文件也是被存放在/etc目录中以服务名称命名的一个目录中了，刘遄老师给同学们整理出了最常用的一些配置参数，大家只需先大致浏览一下即可：

|参数|作用|
|:|
|http_port 3128|监听的端口号。|
|cache_mem 64M|内存缓冲区的大小。|
|cache_dir ufs /var/spool/squid 2000 16 256|硬盘缓冲区的大小。|
|cache_effective_user squid|设置缓存的有效用户。|
|cache_effective_group squid|设置缓存的有效用户组。|
|dns_nameservers IP地址|一般不设置，用服务器默认的DNS地址。|
|cache_access_log /var/log/squid/access.log|访问日志文件的保存路径。|
|cache_log /var/log/squid/cache.log|缓存日志文件的保存路径。|
|visible_hostname linuxprobe.com|设置Squid服务主机的名称。|

#### 正向代理
标准正向代理

Squid服务程序软件包在正确安装并启动后默认就已经可以为用户提供标准正向代理模式服务了，而不需要单独再去修改配置文件或者其他操作。

```
systemctl restart squid
systemctl enable squid
```

如此公开而没有密码验证的代理服务终归觉得不放心，万一有其他人也来“蹭网”代理服务怎么办呢？Squid服务程序默认的会占用3128、3401与4827等端口号，咱们可以把默认占用的端口号修改成其他值，这样应该能起到一定的保护作用吧~同学们都知道在Linux系统配置服务程序就是在修改该服务的配置文件，因此直接在/etc目录中找到和squid服务程序同名目录中的配置文件，把其中http_port参数后面原有3128修改为10000，这样即是把Squid服务程序的代理服务端口修改成了新值，当然最后不要忘记再重启下服务程序哦~

```
vim /etc/squid/squid.conf
http_port 10000

systemctl restart squid
```

有没有突然觉得这一幕似曾相识？在前面的第10章10.5.3小节咱们学习过基于端口号来部署httpd服务程序的虚拟主机功能，当时在编辑完配置文件后重启服务程序时被直接提示报错了，虽然现在重启服务程序并没有直接报错，但其实客户并不能使用代理服务呢。SElinux安全子系统认为Squid服务程序使用3128端口号是理所应当的，默认策略规则中也是允许的，但现在却在尝试使用新的10000端口号，这是原本并不属于Squid服务程序应该使用的系统资源，因此需要手动把新的端口号添加到squid服务程序在SElinux域的允许列表中即可

```
semanage port -l | grep squid_port_t
semanage port -a -t squid_port_t -p tcp 10000
```

#### ACL访问控制
在日常工作中企业员工只有通过公司内部网关服务器才能登陆到互联网中，那么当把Squid服务程序部署成为了公司企业的网关服务器后ACL访问控制列表功能也是非常实用的了，它可以根据指定的策略条件来进行对数据缓存或限制用户的访问操作，而大多数的情况这种限制策略都会十分奏效，很多公司分时段不让员工逛淘宝、打网页游戏都可以通过Squid服务程序的ACL访问控制列表功能实现。如果读者中有想以后做企业的同学，牢记本小节所教的内容，以后可以禁止某几个招聘网站或竞争对手的网站，绝对能够有效降低员工的跳槽几率呢~

ACL访问控制列表功能是由多个规则策略条目组成的，Squid服务程序可以根据指定的条件来允许或限制访问请求，匹配顺序会像防火墙策略一样由上至下，一旦匹配到符合条件的数据则立即执行相应操作并结束匹配过程。为了避免ACL访问控制列表功能把所有流量都禁止或全部流量都允许，也就起不到访问控制的预想效果了，因此运维工程师们通常会在控制列表的最下面部分写上deny all或者allow all来避免安全隐患。为了让同学们更好的理解Squid服务程序提供的ACL访问控制列表功能有多么的强大，刘遄老师接下来通过四个小实验来给大家操作演示，并进行详细的描述。

第1个实验：只允许来IP地址为192.168.10.20的客户主机使用服务器本地Squid服务程序提供的代理服务，禁止其余所有的主机代理请求。配置文件依然是Squid服务程序的主配置文件，但需要留心下配置参数的书写位置，因为如果太靠前的话，有些Squid服务程序自身的语句都没有加载完，就会导致策略不生效的，当然也不用太靠后，大约在26-32行左右就可以，这样分开书写的话会非常便于今后的修改。

```
vim /etc/squid/squid.conf
 8 acl localnet src 10.0.0.0/8 # RFC1918 possible internal network
 9 acl localnet src 172.16.0.0/12 # RFC1918 possible internal network
 10 acl localnet src 192.168.0.0/16 # RFC1918 possible internal network
 11 acl localnet src fc00::/7 # RFC 4193 local private network range
 12 acl localnet src fe80::/10 # RFC 4291 link-local (directly plugged) mac hines
 13 
 14 acl SSL_ports port 443
 15 acl Safe_ports port 80 # http
 16 acl Safe_ports port 21 # ftp
 17 acl Safe_ports port 443 # https
 18 acl Safe_ports port 70 # gopher
 19 acl Safe_ports port 210 # wais
 20 acl Safe_ports port 1025-65535 # unregistered ports
 21 acl Safe_ports port 280 # http-mgmt
 22 acl Safe_ports port 488 # gss-http
 23 acl Safe_ports port 591 # filemaker
 24 acl Safe_ports port 777 # multiling http
 25 acl CONNECT method CONNECT
 26 acl client src 192.168.10.20
 31 http_access allow client
 32 http_access deny all
 33 http_access deny !Safe_ports
 ```
 
 第2个实验：禁止所有客户主机访问网址中包含linux关键词的网站请求，这种ACL访问控制列表功能模式是比较粗矿、暴力的，任何用户访问的网址中只要带有了某个关键词就会被立即禁止，而不影响其他网站的访问请求。
 
 ```
 vim /etc/squid/squid.conf
  8 acl localnet src 10.0.0.0/8 # RFC1918 possible internal network
 9 acl localnet src 172.16.0.0/12 # RFC1918 possible internal network
 10 acl localnet src 192.168.0.0/16 # RFC1918 possible internal network
 11 acl localnet src fc00::/7 # RFC 4193 local private network range
 12 acl localnet src fe80::/10 # RFC 4291 link-local (directly plugged) mac hines
 13 
 14 acl SSL_ports port 443
 15 acl Safe_ports port 80 # http
 16 acl Safe_ports port 21 # ftp
 17 acl Safe_ports port 443 # https
 18 acl Safe_ports port 70 # gopher
 19 acl Safe_ports port 210 # wais
 20 acl Safe_ports port 1025-65535 # unregistered ports
 21 acl Safe_ports port 280 # http-mgmt
 22 acl Safe_ports port 488 # gss-http
 23 acl Safe_ports port 591 # filemaker
 24 acl Safe_ports port 777 # multiling http
 25 acl CONNECT method CONNECT

 acl deny_keyword url_regex -i linux

 31 http_access deny deny_keyword
 33 http_access deny !Safe_ports
 ```
 
 第3个实验：如果是直接禁用了某个网址关键词，那肯定会有一大批网站被误封，这样在工作中就有可能影响了同事们正常的工作，因此咱们是可以直接禁止某个特定的网址的。
 
 ```
 vim /etc/squid/squid.conf
  8 acl localnet src 10.0.0.0/8 # RFC1918 possible internal network
 9 acl localnet src 172.16.0.0/12 # RFC1918 possible internal network
 10 acl localnet src 192.168.0.0/16 # RFC1918 possible internal network
 11 acl localnet src fc00::/7 # RFC 4193 local private network range
 12 acl localnet src fe80::/10 # RFC 4291 link-local (directly plugged) mac hines
 14 acl SSL_ports port 443
 15 acl Safe_ports port 80 # http
 16 acl Safe_ports port 21 # ftp
 17 acl Safe_ports port 443 # https
 18 acl Safe_ports port 70 # gopher
 19 acl Safe_ports port 210 # wais
 20 acl Safe_ports port 1025-65535 # unregistered ports
 21 acl Safe_ports port 280 # http-mgmt
 22 acl Safe_ports port 488 # gss-http
 23 acl Safe_ports port 591 # filemaker
 24 acl Safe_ports port 777 # multiling http
 25 acl CONNECT method CONNECT

acl deny_url url_regex http://www.linuxcool.com

 31 http_access deny deny_url
 33 http_access deny !Safe_ports
 ```

第4个实验：禁止企业内部下载某些后缀的文件，这其实是一件长期让运维人员头疼的事情，因为在企业内网中总是会有些人会偷偷下载东西，要么就是游戏，要么就是回家看的电影，导致其他同事网速特别慢，还有可能影响生产环境的正常运转。这样的话咱们可以禁止所有用户访问rar或mp3等后缀文件的请求，这样能防住不少电脑小白，让他们知难而退，但如果对方是在用迅雷等P2P下载软件的话就只能用专业级WAF应用防护防火墙系统才能禁止了。

 ```
vim /etc/squid/squid.conf
8 acl localnet src 10.0.0.0/8 # RFC1918 possible internal network
 9 acl localnet src 172.16.0.0/12 # RFC1918 possible internal network
 10 acl localnet src 192.168.0.0/16 # RFC1918 possible internal network
 11 acl localnet src fc00::/7 # RFC 4193 local private network range
 12 acl localnet src fe80::/10 # RFC 4291 link-local (directly plugged) mac hines
 13 
 14 acl SSL_ports port 443
 15 acl Safe_ports port 80 # http
 16 acl Safe_ports port 21 # ftp
 17 acl Safe_ports port 443 # https
 18 acl Safe_ports port 70 # gopher
 19 acl Safe_ports port 210 # wais
 20 acl Safe_ports port 1025-65535 # unregistered ports
 21 acl Safe_ports port 280 # http-mgmt
 22 acl Safe_ports port 488 # gss-http
 23 acl Safe_ports port 591 # filemaker
 24 acl Safe_ports port 777 # multiling http
 25 acl CONNECT method CONNECT

acl badfile urlpath_regex -i \.mp3$ \.rar$

 31 http_access deny badfile
 33 http_access deny !Safe_ports
 ```

#### 透明正向代理
正向代理服务一般是面向于企业内部所有成员的，但每个企业成员对电脑的熟悉程度也不全相同，尤其您所在的公司不是IT行业，那想教会同事们使用代理服务还真是一件很麻烦的事情呢，有些时候公司领导也会为了使用ACL访问控制列表功能限制员工在公司内上网的行为，强制要求所有人通过代理服务上网，这时就必须要用透明的正向代理模式了。透明代理技术中的透明指的是让用户在没有感知的情况下就使用了代理服务，这样一方面是不需要用户去手动配置代理服务器的信息，进而降低了代理服务的使用门槛，另一方面也是为了更好的监督员工上网的行为，要想上网就必须通过Squid服务程序代理才行。

透明代理技术是一种普遍不会被用户感知到的代理模式，用户无需在浏览器或其他软件中配置代理服务器地址、端口号等信息，而是直接由DHCP服务器把网卡配置信息分发给客户主机，其中把网关地址指向到Squid服务器即可。这样不论用户是在本地使用浏览器打开网站，还是在QQ上面聊天就都会默认通过代理服务了。

既然要让用户不去配置代理服务器的信息就能使用代理服务，那咱们作为技术的运营商也就必须提前把网络及数据转发功能配置好，需要使用第8章8.3.2小节学习的SNAT源地址转换协议来完成数据的转发，让客户端主机可以把数据交给Squid代理服务器并转发至外网中。简单来说，就是让Squid服务器作为一个中间人，实现内网客户端主机与外部互联网之间数据转发交换，是一个功能丰富的“传话者”，由于还未部署实现SNAT源地址转换协议功能，因此当前内网客户端主机是肯定不能访问外网的。

 ```
iptables -t nat -A POSTROUTING -p udp --dport 53 -o eth0 -j MASQUERADE

echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
sysctl -p
```

与配置DNS和SNAT转发数据相比，Squid服务程序的透明代理模式配置过程就显得十分简单了，只需要在主配置文件中服务器端口号的后面追加上中透明（transparent）的英语单词，然后把约62行的#（井号）注释符去除设置缓存的保存路径就可以了。保存退出配置文件后再分别使用squid -k parse来检查下配置文件是否有错误，以及使用squid -z命令来对Squid服务程序的透明代理技术进行初始化。

```
vim /etc/squid/squid.conf
http_port 3128 transparent
cache_dir ufs /var/spool/squid 100 16 256
```

```
squid -k parse
squid -z
systemctl restart squid
```

配置妥当并重启Squid服务程序没有报错信息后，咱们就可以继续来完成SNAT源地址数据转发功能了，原理其实很简单，就是使用iptables防火墙管理命令把所有客户端主机对网站80端口的请求转发至Squid服务器本地的3128端口上来做转发，具体配置参数如下，这时客户端主机再刷新下浏览器就又能看到网站内容啦。

```
iptables -t nat -A PREROUTING  -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 3128
iptables -t nat -A POSTROUTING -s 192.168.10.0/24 -o eth0 -j SNAT --to 您的桥接网卡IP地址
service iptables save
```

现在肯定有同学在想——如果开启了SNAT源地址转换协议，那么数据不就是直接被转发到外网了吗？是否还依然在使用Squid服务程序提供的代理服务呢？其实只要仔细的看下iptables防火墙命令就会发现，刘遄老师刚刚并不是单纯的开启了SNAT源地址转换功能，而是通过把客户机访问外网80端口的请求转发至了本地服务器的3128端口号上面，从而还是强制用户必须通过Squid服务程序才能上网，为了验证这个说法，咱们可以编辑Squid服务程序的配置文件，单独禁止掉书籍在线学习网站（http://www.linuxprobe.com/），然后刷新客户端网页即可看到又被禁止了。

#### 反向代理
网站页面是由静态资源和动态资源一起组成的网络资源，其中静态资源包括有网站架构CSS文件、大量的图片、视频等数据，这些数据相对比于动态资源来说更加的稳定，一般是不会经常发生改变的数据，而且随着建站技术的更新换代，人们的审美也在不断提升，因此图片内容越来越占据了网站中大面积的空间。如果能够把静态资源从网站页面中抽离出去，然后在全国各地部署静态资源数据的缓存节点，这样不仅能够大幅提升用户访问网站页面的速度，而且当由缓存服务器节点响应处理静态资源请求时，网站源服务器的负载压力问题也会得到明显的改善。

反向代理是Squid服务程序的重要工作模式之一，原理是把一部分用户原本应该向网站源服务器发起的请求交由给Squid缓存服务器节点来处理。但这种技术的弊端也很明显，如果有一个坏心肠的人把自己的域名和服务器反向代理到某个知名的网站上面，这样理论上来讲当用户访问到了这个骇客自己的域名时，也会看到和那个知名网站一摸一样的内容，有些诈骗网站就是这样骗取网民信任的，因此当前许多的网站都已经默认禁止了反向代理，或者开启了CDN内容分发网络的网站也是可以避免这种内容窃取行为的。如果网站开启了防护功能的话一般会看到如图16-15所示的报错信息，刘遄老师在此为了实验需要临时关闭了《Linux就该这么学》书籍在线学习网站的CDN服务及防护插件，请同学们也尽量选择用自己的网站或博客进行实验操作，避免影响到其他网站的正常运转，给别人造成麻烦。

使用Squid服务程序来配置反向代理服务是非常简单的，首先找到一个网站源服务器的IP地址，然后编辑Squid服务程序的主配置文件，把3128端口号修改成服务器地址和端口号，此时正向解析服务会被暂停，不能同时与反向代理一起使用，然后按照下面参数的格式写入要反向代理的网站源服务器IP地址信息，保存退出后重启Squid服务程序即可。

```
vim /etc/squid/squid.conf
http_port 您的桥接网卡IP地址:80 vhost
cache_peer 网站源服务器IP地址 parent 80 0 originserver
```

### 使用OpenLDAP部署目录服务
#### 了解目录服务
回忆前面所学的章节，我们发现其实目录可以被理解成是一种为查询、浏览或搜索的数据库，但数据库又分为了目录数据库和关系数据库，目录数据库主要用于存储较小的信息（如姓名、电话、主机名等），同时具有很好的读性能，但在写性能方面比较差，所以不适合存放那些需要经常修改的数据。

目录服务则是由目录数据库和一套能够访问和处理数据库信息的协议组成的服务协议，用于集中的管理主机帐号密码，员工名字等数据，大大的提升了管理工作效率。轻量级目录访问协议LDAP(Lightweight Directory Access Protocol)是在目录访问协议X.500的基础上研发的，主要的优势是：

>X.500目录协议功能非常臃肿，消耗大量资源，无法做到快速查询且不支持TCP/IP协议网络。

LDAP采用树状结构存储数据（类似于前面学习的DNS服务程序），用于在IP网络层面实现对分布式目录的访问和管理操作，条目是LDAP协议中最基本的元素，可以想象成字典中的单词或者数据库中的记录，通常对LDAP服务程序的添加、删除、更改、搜索都是以条目为基本对象的。

```
dn:每个条目的唯一标识符，如上图中linuxprobe的dn值是：
cn=linuxprobe,ou=marketing,ou=people,dc=mydomain,dc=org

rdn:一般为dn值中最左侧的部分，如上图中linuxprobe的rdn值是：
cn=linuxprobe

base DN:此为基准DN值，表示顶层的根部，上图中的base DN值是：
dc=mydomain,dc=org
```

而每个条目可以有多个属性（如姓名、地址、电话等），每个属性中会保存着对象名称与对应值，LDAP已经为运维人员对常见的对象定义了属性，其中有：

|属性名称|属性别名|语法|描述|值（举例）|
|:|
|commonName|cn|Directory String|名字|sean|
|surname|sn|Directory String|姓氏|Chow|
|organizationalUnitName|ou|Directory String|单位（部门）名称|IT_SECTION|
|organization|o|Directory String|组织（公司）名称|linuxprobe|
|telephoneNumber||Telephone Number|电话号码|911|
|objectClass|||内置属性|organizationalPerson|

#### 目录服务实验
OpenLdap是基于LDAP协议的开源程序，它的程序名称叫做slapd，本次实验需要用到两台主机：

```
主机名称	操作系统	IP地址
LDAP服务端
(instructor.linuxprobe.com)	红帽RHEL7操作系统	192.168.10.10
LDAP客户端	红帽RHEL7操作系统	192.168.10.20
```

##### 配置LDAP服务端
安装openldap与相关的软件包

​```bash
yum install -y openldap openldap-clients openldap-servers migrationtools
```

生成密钥文件（记下生成出的值，后面要用）：

```
slappasswd -s linuxprobe -n > /etc/openldap/passwd

cat /etc/openldap/passwd
{SSHA}v/GJvGG8SbIuCxhfTDVhkmWEuz2afNIR
```

写入一条主机与IP地址的解析记录：

```
echo "192.168.10.10 instructor.linuxprobe.com" >> /etc/hosts
```

因为LDAP目录服务是以明文的方式在网络中传输数据的（包括密码），这样真的很不安全，所以我们采用TLS加密机制来解决这个问题，使用openssl工具生成X509格式的证书文件（有效期为365天）。

```
openssl req -new -x509 -nodes -out /etc/openldap/certs/cert.pem -keyout /etc/openldap/certs/priv.pem -days 365

必须输入的选项
Common Name (eg, your name or your server hostname) []:instructor.linuxprobe.com
```

修改证书的所属与权限

```
cd /etc/openldap/certs/
chown ldap:ldap *
chmod 600 priv.pem
```

复制一份LDAP的配置模板

```
cp /usr/share/openldap-servers/DB_CONFIG.example /var/lib/ldap/DB_CONFIG
```

生成数据库文件（不用担心报错信息）

```
slaptest
```

修改LDAP数据库的所属主与组

```
chown ldap:ldap /var/lib/ldap/*
```

启动slapd服务程序并设置为开机启动

```
systemctl restart slapd
systemctl enable slapd
```

在LDAP目录服务中使用LDIF(LDAP Interchange Format)格式来保存信息，而LDIF是一种标准的文本文件且可以随意的导入导出，所以我们需要有一种“格式”标准化LDIF文件的写法，这中格式叫做“schema”，schema用于指定一个目录中所包含对象的类型，以及每一个类型中的可选属性，我们可以将schema理解为面向对象程序设计中的“类”，通过“类”定义出具体的对象，因此其实LDIF数据条目则都是通过schema数据模型创建出来的具体对象：

ldapadd命令用于将LDIF文件导入到目录服务数据库中，格式为：“ldapadd [参数] LDIF文件”。

|参数|作用|
|:|
|-x|进行简单认证。|
|-D|用于绑定服务器的dn。|
|-h：|目录服务的地址。|
|-w：|绑定dn的密码。|
|-f：|使用LDIF文件进行条目添加的文件。|

添加cosine和nis模块

```
cd /etc/openldap/schema/

ldapadd -Y EXTERNAL -H ldapi:/// -D "cn=config" -f cosine.ldif
ldapadd -Y EXTERNAL -H ldapi:/// -D "cn=config" -f nis.ldif
```

创建/etc/openldap/changes.ldif文件，并将下面的信息复制进去（注意有一处要修改的地方）

```
vim /etc/openldap/changes.ldif
olcRootPW: 此处输入之前生成的密码（如{SSHA}v/GJvGG8SbIuCxhfTDVhkmWEuz2afNIR）
```

将新的配置文件更新到slapd服务程序：

```
ldapmodify -Y EXTERNAL -H ldapi:/// -f /etc/openldap/changes.ldif
```

创建/etc/openldap/base.ldif文件，并将下面的信息复制进去。

```
vim /etc/openldap/base.ldif
dn: dc=linuxprobe,dc=com
dc: linuxprobe
objectClass: top
objectClass: domain

dn: ou=People,dc=linuxprobe,dc=com
ou: People
objectClass: top
objectClass: organizationalUnit

dn: ou=Group,dc=linuxprobe,dc=com
ou: Group
objectClass: top
objectClass: organizationalUnit
```

创建目录的结构服务

```
ldapadd -x -w linuxprobe -D cn=Manager,dc=linuxprobe,dc=com -f /etc/openldap/base.ldif
```

创建测试用的用户

```
useradd -d /home/ldap ldapuser
```

设置帐户的迁移

```
vim /usr/share/migrationtools/migrate_common.ph
$DEFAULT_MAIL_DOMAIN = "linuxprobe.com";
$DEFAULT_BASE = "dc=linuxprobe,dc=com";
```

将当前系统中的用户迁移至目录服务

```
cd /usr/share/migrationtools/

grep ":10[0-9][0-9]" /etc/passwd &gt; passwd

./migrate_passwd.pl passwd users.ldif

ldapadd -x -w linuxprobe -D cn=Manager,dc=linuxprobe,dc=com -f users.ldif
```

将当前系统中的用户组迁移至目录服务

```
cd /usr/share/migrationtools/

grep ":10[0-9][0-9]" /etc/group &gt; group

./migrate_group.pl group groups.ldif

ldapadd -x -w linuxprobe -D cn=Manager,dc=linuxprobe,dc=com -f groups.ldif
```

测试linuxprobe用户的配置文件

```
ldapsearch -x cn=ldapuser -b dc=linuxprobe,dc=com
```

设置ldap日志文件
```
vim /etc/rsyslog.conf
local4.* /var/log/ldap.log
systemctl restart rsyslog
```

安装httpd服务程序

```
yum install -y httpd
cp /etc/openldap/certs/cert.pem /var/www/html
systemctl restart httpd
systemctl enable httpd
```

##### 配置LDAP客户端
将LDAP服务端主机名与IP地址的解析记录写入：

```
echo "192.168.10.10 instructor.linuxprobe.com" >> /etc/hosts
```

安装相关的软件包

```
yum -y install openldap-clients nss-pam-ldapd authconfig-gtk pam_krb5
```

运行系统认证工具，并填写LDAP服务信息：
```
system-config-authentication
```

稍等片刻后，验证本地是否已经有了ldapuser用户

```
id ldapuser
```

##### 自动挂载用户目录
虽然在客户端已经能够使用LDAP验证帐户了，但是当切换到ldapuser用户时会提示没有该用户的家目录：

```
su - ldapuser
su: warning: cannot change directory to /home/ldapuser: No such file or directory
mkdir: cannot create directory '/home/ldapuser': Permission denied
```

原因是本机并没有该用户的家目录，我们需要配置NFS服务将用户的家目录自动挂载过来：

在LDAP服务端添加共享信息

```
vim /etc/exports
/home/ldap 192.168.10.20 (rw,sync,root_squash)

#重启nfs-server
systemct restart nfs-server
```

在LDAP客户端查看共享信息

```
showmount -e 192.168.10.10
```

将共享目录挂载到本地：

```
mkdir /home/ldap
mount -t nfs 192.168.10.10:/home/ldap /home/ldap
```

再次尝试切换到ldapuser用户，这样非常顺利：

```
[root@linuxprobe ldap]# su - ldapuser
Last login: Tue Oct  6 11:51:25 CST 2015 on pts/3
[ldapuser@linuxprobe ~]$
```

设置为开机自动挂载：

```
vim /etc/fstab
192.168.10.10:/home/ldap /home/ldap nfs defaults 0 0
```

## 第十七节课
### 使用iSCSI服务部署网络存储
#### iSCSI技术介绍
硬盘是计算机硬件设备中重要的组成部分之一，存储设备的IO读写速度快慢也直接影响着服务器整体性能的高低，咱们在书籍第6章、第7章分别细致的学习了硬盘存储结构、RAID磁盘阵列技术及LVM逻辑卷管理技术等等存储设备技术，这些技术无论是软件还是硬件层面的，大多都是在努力解决硬盘存储设备的读写慢快问题或保障存储内容数据安全。为了能够让存储设备更加强大，一百年来人类一直在努力发挥强大的创造力来不断改进着物理硬盘设备的接口协议，当前硬盘接口类型主要有IDE、SCSI和SATA三种。SCSI小型计算机系统接口是一种计算机与硬盘或光驱等设备系统级接口最常见的标准协议，在当今存储设备上最常使用的一种接口协议，SCSI接口协议的出现终结了IDE接口时代，人们再也不用忍受见到数据传输很慢的情况了。

不论是使用什么类型的硬盘接口，硬盘数据总是要通过计算机主板上的总线来跟CPU处理器、内存设备进行数据交换传输，这样物理环境的限制大大增加了硬盘资源共享的不便捷性，当时人们肯定在想如果能够把硬盘存储资源也能放到互联网上那该多方便啊，于是IBM公司便开始动手研发基于TCP/IP网络协议和SCSI接口协议的新型存储技术，也就是目前所看到的iSCSI小型计算机系统接口（Internet Small Computer System Interface）。这是一种能够把SCSI接口与以太网技术相结合的新型存储技术，基于iSCSI协议便能在网络中传输SCSI接口的命令和数据内容了，这样不仅克服了传统SCSI接口设备的物理局限性，实现让机房之间可以跨越省市来共享存储设备资源，还可以做到在不停机的状态下扩展存储容量。

刘遄老师认为同学们不仅应该学习好这项技术，更要知道iscsi存储技术在生产环境中的优势和缺点，这样在工作中才能灵活使用。首先来讲，使用iscsi存储技术确实非常便捷，从存储资源获取形式上发生了很大的变化，解脱了物理环境的限制，同时还能够把存储资源分开给多个服务器一起使用，是一种非常推荐的网络存储技术。但iscsi存储协议的技术局限性也十分明显，那就是网速！以前硬盘设备是直接通过主板上的总线进行数据传输，但当前则需要让互联网来作为数据传输的载体，在传输速率上以及稳定性上面都是当前遇到的瓶颈问题，随着互联网和网络技术的持续发展，相信iscsi技术在未来也会被不断得到改善的。

对了，还有一个很重要的问题，既然咱们要通过以太网来传输硬盘数据，那数据是通过网卡传入到电脑中的吗？这就有必要向同学们介绍下iSCSI-HBA卡了，一般的网卡是连接的网络总线和内存，是用来供计算机上网的，而iSCSI-HBA卡则是连接的SCSI接口或FC总线和内存，是专门供主机之间交换存储数据的，协议也有本质的区别，iSCSI-HBA卡如图17-1所示，Linux系统服务器会基于iSCSI协议把SCSI硬盘设备、命令与数据打包成标准的TCP/IP数据包，然后通过以太网传输到目标的存储设备，而当远端的计算机系统接收到这些数据包后还需要基于iSCSI协议把TCP/IP数据包解压成SCSI设备、命令与数据。

#### 创建RAID磁盘阵列
既然要使用iSCSI技术为远程用户提供硬盘存储资源，那首先应该保证服务器存储资源的稳定性和可用性，否则一旦远程用户在使用过程中出现故障，那排错维修起来的难度相比较本地硬盘设备来讲是要复杂困难很多的。

使用mdadm命令创建RAID磁盘阵列组，-Cv参数为创建阵列组并显示过程，/dev/md0为生成的阵列组名称，-n 3参数为创建RAID5级别磁盘阵列组所需要用的硬盘个数，-l 5参数为RAID磁盘阵列组的级别，-x 1参数为磁盘阵列组的热备盘个数，在命令的最后面逐一写上要使用的硬盘名称。

```
mdadm -Cv /dev/md0 -n 3 -l 5 -x 1 /dev/sd{b,c,d,e}
```

命令执行成功后就得到了一块名称为/dev/md0的新设备，这是一块RAID5级别的磁盘阵列组，并且还有一块热备盘为硬盘数据保驾护航，可使用mdadm -D命令参数来查看设备的详细信息。另外由于使用远程设备时极有可能出现识别顺序发生变化的情况，如果直接在fstab挂载配置文件中写/dev/sdb、/dev/sdc等设备名称的话，就有可能下一次挂载错了存储设备，UUID值为设备的唯一标识符 ，是一种区分本地或远程设备最精准的方法，咱们需要把这段字符串记录下来，一会填写到挂载配置文件中。

```
mdadm -D /dev/md0
UUID : 3370f643:c10efd6a:44e91f2a:20c71f3e
```

#### 配置iSCSI服务端

iSCSI技术在工作形式上分为服务端（target）与客户端（initiator），iSCSI服务端即用于存放硬盘存储资源的服务器，作为前面创建RAID磁盘阵列组的存储端，能够为用户提供可用的存储资源。而iSCSI客户端则是用户使用的软件，用于获取远程服务端的存储资源。

```
yum -y install targetd targetcli
```

安装完成后启动iSCSI的服务端targetd程序，然后把服务程序加入到开机启动项中，以便下次重启服务器后依然能够为用户提供iSCSI共享存储资源服务。

```
systemctl start targetd
systemctl enable targetd
```

第2步：配置iSCSI服务端共享资源，targetcli是用于管理iSCSI服务端存储资源的专用配置命令，它能够提供类似于前面学习过的fdisk命令的交互式配置功能，将iSCSI共享资源的配置内容抽象成了“目录”的形式，咱们只需要把各类配置信息写入到对应“目录”中即可。难点主要集中在认识每个“参数目录”的作用，当把配置参数正确妥当的填写到“目录”中后，iSCSI服务端也就能够提供存储设备资源服务了，总体来说在红帽RHEL7系统中配置iSCSI共享资源相比较5和6版本变得很简单了。

执行targetcli命令后就能看到交互式的配置界面了，这里面可以使用很多的Linux命令，利用使用ls查看参数目录的结构，或者使用cd来进行切换也是可以的。/backstores/block是iSCSI服务端对共享设备的配置位置，需要把刚刚创建的RAID5磁盘阵列组md0文件加入到资源池中并重新命名为disk0，这样用户不会知道是由服务器那块硬盘提供的共享存储资源，而只会看到叫做disk0的存储设备。

```
targetcli

ls
cd /backstores/block
create disk0 /dev/md0
```

第3步：创建iSCSI target名称及配置共享资源，iSCSI target名称是由系统自动生成的，这是一串用于描述共享资源的相对唯一的字符串，稍后用户扫描发现iSCSI服务端时可看到这串共享资源的信息，因此系统生成后同学们不需要记住它们。名称生成后还会在叫做/iscsi的参数目录中创建一个与其同名的新“目录”用来存放共享资源，咱们需要把刚刚加入到iSCSI共享资源池中的硬盘设备添加到此处，这样稍后用户登陆iSCSI服务端时即可默认使用这个设备提供的共享存储资源了。

```
targetcli

cd iscsi
create
cd iqn.2003-01.org.linux-iscsi.linuxprobe.x8664:sn.d497c356ad80
cd tpg1/luns
create /backstores/block/disk0 
```

第4步：设置ACL访问控制列表，iSCSI协议是通过客户端名称进行验证的，也就是说用户获取存储共享资源的时候不需要输入密码，而只要iSCSI客户端的名称与服务端中设置的ACL访问控制列表名称一致即可，因此需要在iSCSI服务端的配置文件中写入一串能够验证用户信息的名称，刘遄老师推荐直接在刚刚系统生成的iSCSI target后追加上类似于:client的参数即可，这样既能保证用户名称具有唯一性，又非常便于管理和阅读：

```
targetcli

cd iscsi
cd iqn.2003-01.org.linux-iscsi.linuxprobe.x8664:sn.d497c356ad80
cd tpg1/acls
create iqn.2003-01.org.linux-iscsi.linuxprobe.x8664:sn.d497c356ad80:client
```

第5步：设置iSCSI服务端的监听IP地址和端口号，因为在生产环境的服务器上面可能有多块网卡，那么到底由那个网卡或IP地址对外提供共享存储资源呢？这就需要咱们手工的在配置文件中定义iSCSI服务器的信息，即在portals参数目录中写上服务器的IP地址即可，接下来会由系统自动提醒主机192.168.10.10的3260端口将向外提供iSCSI共享存储资源服务

```
targetcli

cd iscsi
cd iqn.2003-01.org.linux-iscsi.linuxprobe.x8664:sn.d497c356ad80
cd tpg1/portals
create 192.168.10.10
```

第6步：配置妥当后检查配置信息，重启iSCSI服务端程序并配置防火墙策略，在配置妥当参数文件后可大致浏览下刚刚配置的信息，同学们也应该跟下面的信息基本保持一致。确认信息无误后可输入exit命令来退出配置命令，但此处切记不要习惯性的按ctrl+c来结束进程，这样的话配置文件是没有被保存的，也就前功尽弃了。最后重启一下iSCSI服务端程序，再设置firewalld防火墙对3260/Tcp端口号的流量放行策略，也就完成了iSCSI服务端全部的配置步骤了。

```
systemctl restart targetd

firewall-cmd --permanent --add-port=3260/tcp
firewall-cmd --reload 
```

#### 配置Linux客户端
现在已经有了很多的Linux服务配置经验，可以总结来说不论是什么服务，客户端的配置步骤都会比服务端更简单一些，在红帽RHEL7系统中iSCSI客户端服务程序initiator已经被默认安装好了，而如果您在生产环境中没有默认安装的话，可以用yum软件仓库来手工安装下

```
yum install iscsi-initiator-utils 
```

如前面所提到的iSCSI协议是通过客户端名称进行验证的，而该名称也就是iSCSI客户端的唯一标识，这串信息必须与服务端配置文件中ACL访问控制列表中的信息匹配一致，否则客户端在尝试使用存储共享时就会弹出验证失败的报错信息。编辑在iSCSI客户端中的initiator名称文件，把刚刚服务端的ACL访问控制列表名称填写进入，然后重启一下客户端iscsid服务程序并加入到开机启动项中

```
vim /etc/iscsi/initiatorname.iscsi
InitiatorName=iqn.2003-01.org.linux-iscsi.linuxprobe.x8664:sn.d497c356ad80:client

systemctl restart iscsid
systemctl enable iscsid
```

在iSCSI客户端使用共享存储资源的步骤很简单，只需要记住刘遄老师的一个小口诀“先发现，再登陆，最后挂载并使用”。iscsiadm是用于管理、查询、插入、更新或删除iSCSI数据库配置文件的命令行工具，用户需要先使用这个iscsiadm命令对远程iSCSI服务端进行扫描发现，查看该服务器上面有那些可用的共享存储资源，-m discovery参数为定义操作目的是扫描发现可用存储资源，-t st参数为扫描发现操作的类型，-p 192.168.10.10参数为对方iSCSI服务端的IP地址

```
iscsiadm -m discovery -t st -p 192.168.10.10
```

使用iscsiadm命令扫描发现到远程iSCSI服务端上可用的存储资源名称后，然后就要进行登陆了，-m node参数为将本机作为一台节点服务器，-T  iqn.2003-01.org.linux-iscsi.linuxprobe.x8664:sn.d497c356ad80参数为要使用的存储资源名称，字符串很长手打容易错，同学们直接复制上面扫描发现到的结果即可，-p 192.168.10.10参数依然为对方iSCSI服务端的IP地址，最后使用--login或-l参数进行登陆验证吧

```
iscsiadm -m node -T iqn.2003-01.org.linux-iscsi.linuxprobe.x8664:sn.d497c356ad80 -p 192.168.10.10 --login
```

在对iSCSI服务端进行登陆验证显示顺利成功后就会在系统中多了一块名为/dev/sdb的设备文件，还记得第6章中刘遄老师说过udev服务在命名硬盘名称的时候与插槽是没有关系的吧~这个远程的存储资源设备文件就这么活生生的出现到了iSCSI客户端主机上面，接下来可以像使用自己电脑上的硬盘一样来对这块设备进行操作了。

```
file /dev/sdb 
```

接下来可以按照之前所学习过的硬盘标准流程进行操作，这部分内容同学们应该早已烂熟于胸，如果忘记的话可以翻回到第6章再复习一下，由于这块存储资源本身也就只有40GB容量了，因此也就不进行分区步骤了，可以直接进行格式化并挂载使用

```
mkfs.xfs /dev/sdb
mkdir /iscsi
mount /dev/sdb /iscsi
df -h
```

每个实验做成功后都会感到像解出一道数学题似得喜悦吧~接下来这块硬盘设备就像在客户端主机本地的硬盘设备一样工作，但有一点需要同学们格外的留意，就是由于udev服务对硬盘设备的命名规则是按照系统识别顺序操作的，那么当客户端主机同时使用多个远程存储资源时，如果下一次识别远程共享设备的顺序发生了变化，那么客户端所挂载目录内的文件可能也会随之混乱。所以为了防止此类问题出现，咱们应该在/etc/fatab配置文件中使用设备的UUID唯一标识符进行挂载，这样不论远程设备资源识别的顺序再怎样发生变化，系统也一样能马上找到设备所对应的正确目录。blkid命令用于查看设备的名称、文件系统及UUID内容，可以用第3章中学习的管道符进行过滤显示出针对/dev/sdb设备的内容信息

```
blkid | grep /dev/sdb
```

最最最最刘遄老师还要再啰嗦一句，由于/dev/sdb是一块网络存储设备，而iSCSI协议是基于TCP/IP网络协议进行传输数据的，因此必须在/etc/fstab配置文件中添加上_netdev参数，代表当系统联网启动后再进行挂载操作，避免系统开机时间过长或启动失败：

```
vim /etc/fstab
UID=eb9cbf2f-fce8-413a-b770-8b0f243e8ad6 /iscsi xfs defaults,_netdev 0 0
```

对了，如果同学们不再需要使用iSCSI共享设备资源了，可以用iscsiadm命令的-u参数来将其设备卸载掉

```
iscsiadm -m node -T iqn.2003-01.org.linux-iscsi.linuxprobe.x8664:sn.d497c356ad80 -u
```

#### [配置Windows客户端](http://www.linuxprobe.com/chapter-17.html#175_Windows)

### 使用MariaDB数据库管理系统
#### 数据库管理系统
数据库是指按照某些特定结构来存储数据资料的软件仓库程序，在当今这个互联网及大数据技术迅速崛起的疯狂时代，在人们生活中也无时无刻都在接触着海量的数据信息，数据库技术已经从最初只能够存储简单表格发展到了存储海量数据的大型分布式模式。尤其是在信息化社会环境下，能够充分有效地管理和利用各类信息资源，是进行科学研究和决策管理的重要前提条件，同时现在数据库技术也是管理信息系统、办公自动化系统、决策支持系统等各类信息系统的核心组成部分，是进行科学研究和决策管理的重要技术手段。

数据库管理系统是一种能够对数据库内容进行建立、修改、删除、查找、维护等等操作的软件程序，数据库管理系统通过把计算机中具体的物理数据转换成适合用户理解的抽象逻辑数据，有效降低管理数据库的技术门槛，因此即便是学习Linux运维方向的工程师也能够对数据库内容进行基本的管理操作。但刘遄老师有必要提醒同学们您手中的这本《Linux就该这么学》书籍的技术主线依然是学习Linux系统的运维技术，数据库管理系统的学习是在主线之上的不断扩展内容之一，不能指望用一两天时间就能精通某门计算机语言或某项技术，因此如果学习本章节内容后对数据库管理系统技术变得兴趣十足，那么就为自己再定下一个长远学习目标。

Mysql是一款当前市场占有率非常高的数据库管理系统，它的技术十分成熟，配置步骤相对简单、并且具有良好的可扩展性，但由于2009年Sun公司被收购了，Mysql项目也被转入到了oracle公司中变成了一项虽然开源、但却又存在多项专利封闭的软件程序。咱们在书籍第0章中介绍过开源软件是全球黑客、极客、程序员等技术高手在开源社区的大旗下的公共智慧结晶，自己的劳动成果被其他公司商业化自然也伤了一大批开源工作者的心，因此由Mysql项目创始者重新研发了一款名为MariaDB的全新数据库管理系统，这款软件当前是由开源社区维护的分支产品，几乎完全兼容Mysql数据库管理系统。同时由于各大公司之间也存在着竞争关系或利益关系，因此在Mysql软件被收购后逐渐从开源陷入封闭的过程中，已经有类似于谷歌公司、维基百科公司等等技术领袖决定转移Mysql数据库的业务到MariaDB数据库，Linux开源系统的领袖厂商红帽 公司也已经决定把RHEL7系统、Centos7系统以及最新Fedora系统的全线产品的默认数据库管理系统定为mariadb，随后还有大量如Opensuse、Slackware等等数十个常见Linux系统做出了同样的表态。

但坦白讲，虽然谷歌、维基百科及红帽公司这样的行业巨头都已经决定采用了最新的MariaDB数据库管理系统，但这并不意味着就比Mysql有明显的优势了，刘遄老师用了将近两周的时间亲测了MariaDB与Mysql的区别，并进行了多项性能测试，认为其实并没有像媒体说的那样有了明显优势。在性能上MariaDB与Mysql基本是保持一致的，而操作命令上更是十分相似，不夸张的说，当咱们学会了MariaDB数据库的命令后，在今后的工作中遇到遇到了Mysql数据库也能够轻松上手。

#### 初始化mariaDB服务
MariaDB数据库管理系统相比于Mysql数据库管理系统有了很多新鲜的扩展特性，例如对微秒级别的支持、线程池、子查询优化、进程报告等等技术，决定学习MariaDB数据库管理系统绝对是一个非常正确的选择。安装部署MariaDB数据库主程序及服务端程序并加入到开机启动。

```
yum install mariadb mariadb-server
```

确认MariaDB数据库软件程序安装完毕并启动成功后请不要立即使用，为了确保数据库的安全性和正常运转，咱们需要先进行对数据库程序初始化操作。这个过程需要经历五个步骤，首先需要让用户来设置root用户在数据库中的密码值，但需要注意该密码并非root管理员用户在系统中的密码，因此默认密码值应该为空，直接回车即可。然后设置root用户在数据库中的专有密码，然后是一次删除匿名帐户以及进行root管理员帐户从远程登陆数据库，这样做能够很有效的保证数据库上运行业务的安全性，然后是删除默认的测试数据库，并取消对其测试数据库的一系列访问权限，最后是刷新授权表，让初始化的设定立即生效。对于上面所谈到的数据库初始化步骤，刘遄老师已经在下面输出信息旁边又进行了简单注释，同学们可以更直观的明白要输入的内容：

```
mysql_secure_installation 


Enter current password for root (enter for none): 当前数据库密码为空，直接敲击回车。

root user without the proper authorisation.
Set root password? [Y/n] y

New password: 输入要为root用户设置的数据库密码。
Re-enter new password: 重复再输入一次密码。

Remove anonymous users? [Y/n] y（删除匿名帐号）

Disallow root login remotely? [Y/n] y(禁止root用户从远程登录)

Remove test database and access to it? [Y/n] y(删除test数据库并取消对其的访问权限)

Reload privilege tables now? [Y/n] y(刷新授权表，让初始化后的设定立即生效)
```

很多生产环境中需要使用站库分离的技术，因此如果同学们需要让root管理员帐户能够用远程访问数据库时，可在刚刚初始化过程中设置允许root管理员帐户从远程访问的策略，然后再设置防火墙允许对本机mysql服务程序的访问请求即可

```
firewall-cmd --permanent --add-service=mysql
firewall-cmd --reload
```

一切就绪!~快来尝试初次登陆到您的MariaDB数据库中吧，分别用-u参数来指定用超级管理员root用户来登陆，而-p参数作用是验证该用户的密码值

```
mysql -u root -p
```

同学们最不习惯的地方一定是每次执行数据库命令后都要用;（分号）结尾，这应该也是与Linux命令最显著的区别的，每条数据库命令后面都要加上结束符，一定要记住并且慢慢习惯这种设定哦~例如可以尝试查看下当前数据库管理系统都有那些数据库

```
show databases;
```

小试牛刀后感觉如何呢？接下来用数据库命令修改下超级管理员root用户在数据库管理系统中的密码值为linuxprobe吧，这样退出后再尝试登陆，就会被提示访问失败啦~

```
set password = password('linuxprobe')
exit
```

#### 管理用户以及授权
在生产环境中总不能一直“死啃”root管理员帐户吧~为了保证数据库系统的安全性，以及让其他工程师人员协同管理数据库内容，咱们可以在MariaDB数据库管理系统中为他们创建出多个数据库专用的帐户，然后再进行合理的权限分配，这样还一定能大大的提升工作效率呢。登陆数据库管理系统后便可以按照CREATE USER 用户名@主机名 IDENTIFIED BY '密码';的格式来创建出数据库专用帐号信息，同学们一定要切记每条数据库命令后面还有个不能省略的;（分号）呦~

```
create user luke@localhost IDENTIFIED BY 'linuxprobe';
```

帐户信息可以使用select命令语句来进行查询，下面代码查询的是该用户的主机名称、帐户名称以及经过加密的密码值信息：

```
use mysql
select host,user,password from user where user="luke";
```

数据库管理系统中的命令一般是比较复杂的，比如给用户进行授权而使用的grant命令吧，它就要求在使用时需要写上要赋予的权限，数据库及表单名称，以及对应的用户及主机信息，但实际只要一旦理解了每个字段的功能含义，其实命令复杂一些也不难懂，GRANT授权命令的常见格式如下表：

|命令|作用|
|:|
|GRANT 权限 ON 数据库.表单名称 TO 用户名@主机名|对某个特定数据库中的特定表单给予授权。|
|GRANT 权限 ON 数据库.* TO 用户名@主机名|对某个特定数据库中的所有表单给予授权。|
|GRANT 权限 ON *.* TO 用户名@主机名|对所有数据库及所有表单给予授权。|
|GRANT 权限1,权限2 ON 数据库.* TO 用户名@主机名|对某个数据库中的所有表单给予多个授权。|
|GRANT ALL PRIVILEGES ON *.* TO 用户名@主机名|对所有数据库及所有表单给予全部授权，（谨慎操作）。|

普通用户当然不能够随便给自己授权啦，就像给每位公民都赠送了一台印钞机，那人们肯定就不会再继续踏实工作了，人类社会协作关系也就会瞬间崩塌啦。所以在做这种高级操作前需要先登陆到root管理员帐户身份上来，然后再进行授权操作，给予luke用户对mysql数据库中user表单的查询、更新、删除及插入权限

```
use mysql;
GRANT SELECT,UPDATE,DELETE,INSERT on mysql.user to luke@localhost;
```

授权操作执行后来查看下luke用户的权限吧

```
show grants for luke@localhost;
```

取消luke用户对mysql数据库中user表单的查询、更新、删除及插入权限

```
use mysql;
revoke SELECT,UPDATE,DELETE,INSERT on mysql.user from luke@localhost;
```

#### 创建数据库与表单
MariaDB数据库管理系统最重要的作用之一就是能够管理数据库及表单内容，一个数据库中可以存放多个数据表，数据表是数据库中最实质的内容，咱们可以定义独一无二的数据库表结构，然后合理的存放数据内容，这样在今后的维护、修改时都会体验到十分便捷。

数据库命令及对应的作用总结

|用法|作用|
|:|
|CREATE database 数据库名称|创建新的数据库。|
|DESCRIBE 表单名称;|描述表单。|
|UPDATE 表单名称 SET attribute=新值 WHERE attribute > 原始值;|更新表单中的数据。|
|USE 数据库名称;	|指定使用的数据库。|
|SHOW databases;|显示当前已有的数据库。|
|SHOW tables;|显示当前数据库中的表单。|
|SELECT * FROM 表单名称;|从表单中选中某个记录值。|
|DELETE FROM 表单名 WHERE attribute=值;|从表单中删除某个记录值。|

想创建数据表单，就要先切换到某个指定的数据库中，例如可以在新建的linuxprobe数据库中创建一个叫做mybook的表单。表单初始化即需要定义存储数据内容的结构，咱们分别定义三个字段项，能够存储15个字符的name字段是用来保存书籍名称的，而整数类型的price与pages则分别是一会存储书籍价格和页数用的~这样当创建命令执行成功后，即可看到表单的结构信息啦

```
create database linuxprobe;
use linuxprobe;
create table mybook (name char(15),price int,pages int);
describe mybook;
```

#### 管理表单及数据

接下来向刚刚创建的mybook数据库表单写入一条书籍信息，插入数据库内容要使用insert命令，写清表单名称以及对应的字段项目，就可以插入一条名称叫做linuxprobe的书籍啦，它的价格和页数分别是60元和518页。命令执行后也就意味着书籍信息已经被写入成功了，咱们便可以来查询表单中的内容啦，使用select命令查询表单内容的时候需要加上想要查询的字段，如果想查看表单中的所有内容也可以干脆使用*（星号）通配符来显示所有内容

```
INSERT INTO mybook(name,price,pages) VALUES('linuxprobe','60','518');
select * from mybook;
```

数据库运维人员讲究四门功课——增、删、改、查，这意味着创建和插入数据库表单内容仅仅是第一步，还需要进一步学习数据库表内容的修改方法，例如咱们可以用update命令把刚刚插入的书籍价格修改为55元，然后再用select命令指定查看下书籍的名称和价格信息，不要再用*（星号）通配符显示所有内容啦

```
update mybook set price=55 ;
select name,price from mybook;
```

同学们还可以用delete命令来删除某个表单中的内容，例如可以清空删除mybook表单中的所有内容，这样再来查询的话也看不到书籍信息啦：

```
delete from mybook;
```

一般数据库表单中都会有成千上万条的数据条目，例如刚刚创建用于保存书籍信息的mybook表单，如果经过时间的推移里面的书籍信息也会变得越来越多，那么如果只是想查看售卖价格大于某个价格的书籍时又该如何定义查询语句呢？咱们先使用刚刚学习过的insert插入命令来依次插入4条书籍信息：

```
INSERT INTO mybook(name,price,pages) VALUES('linuxprobe1','30','518');

INSERT INTO mybook(name,price,pages) VALUES('linuxprobe2','50','518');

INSERT INTO mybook(name,price,pages) VALUES('linuxprobe3','80','518);

INSERT INTO mybook(name,price,pages) VALUES('linuxprobe4','100','518');
```

要想让查询结果更加精准，那么就需要把select结合where命令来一起使用了，where是用于在数据库中进行匹配查询的条件命令，咱们可以设置一个查询的条件，那么就仅会查找出符合该条件的数据内容，常用的参数包括有：

|参数|作用|
|:|
|=|相等。|
|<>或!=|不相等。|
|>|大于。|
|<|小于。|
|>=|大于或等于。|
|<=|小于或等于。|
|BETWEEN|在某个范围内。|
|LIKE|搜索一个例子。|
|IN|在列中搜索多个值。|

分别在mybook表单中查找出价格大于75元或价格不等于80元的书籍，这两个查询条件练熟之后还可以再自己试试挑战下精准查询名称为linuxprobe2的书籍信息吧~

```
select * from mybook where price>75;
select * from mybook where price!=80;
```

#### 数据库的备份及恢复
由于咱们的技术主线是成为Linux系统运维工程师，因此在学完上面这些数据库管理命令后能够对数据库管理系统有一定的了解就已经足够了，另外数据库的备份及恢复也是比较实用的技术。mysqldump命令用于备份数据库数据，格式为：“mysqldump [参数] [数据库名称]，其中参数与mysql命令大致相同，-u参数用于定义登陆数据库的用户名称，而-p参数代表密码提示符。例如接下来把linuxprobe数据库内容导出成一个文件保存到root管理员用户的家目录中：

```
mysqldump -u root -p linuxprobe > /root/linuxprobeDB.dump
```
进入到MariaDB数据库管理系统中把linuxprobe数据库完整的删除掉再重新建立，这样mybook数据库表单也将被彻底的清空掉

```
drop database linuxprobe;
show databases;
create database linuxprobe;
```

接下来就是见证数据恢复效果的时刻啦，使用输入重定向符把刚刚备份的数据库文件导入到mysql命令中，命令执行成功后再登陆到MariaDB数据库中就会发现又能看到数据库以及表单了，恢复数据库内容成功！

```
mysql -u root -p linuxprobe < /root/linuxprobeDB.dump

mysql -u root -p
use linuxprobe;
show tables;
describe mybook;
```

## 第十八节课
### 使用PXE+Kickstart无人值守安装服务
#### 无人值守系统
虽然刘遄老师在本书籍第1章中教过同学们用光盘镜像来安装Linux系统的方法，但坦白来讲，如果生产环境中有数百台服务器需要安装系统，那么这种光盘镜像的安装方式就显得实在效率太低了，况且还要为这数百台服务器购买数百张的系统安装光盘或U盘系统，然后还必须对每台系统设置安装初始化向导，这种重复性极高又无聊的事情可能会白白浪费掉咱们一整天的时间，是不是想想都觉得痛苦了呢。

其实当生产环境中出现上百台服务器需要安装系统的时候，可以用PXE+TFTP+VSftp+DHCP+Kickstart服务来整合部署出一个无人值守安装系统服务来。这种无人值守安装系统服务可以实现自动化的完成对数十台服务器自动安装系统的工作，有效的避免了运维工作人员重复性的工作，进而也大大的提高了工作效率。

PXE预启动执行环境(即Preboot execute environment)是一种能够让计算机通过网络启动的引导方式，只要网卡支持PXE协议即可使用，用于在无人值守安装系统服务中引导客户机安装服务。Kickstart是一种无人值守的安装方式，工作原理就是预先把原本需要运维人员手工填写的参数保存成一个ks.cfg文件，当安装过程中出现需要填写参数时则自动匹配Kickstart生成的文件，所以只要Kickstart文件包含了安装过程中所有需要人工填写的参数，那么理论上来讲运维人员就完全不需要再进行手工操作，喝着咖啡等待安装完毕即可。

其中TFTP、Vsftpd以及DHCP服务程序的配置部署方法咱们已经分别在书籍中的第11章和第14章进行了详细讲解~由于对方客户端主机当前并没有完整的操作系统，因此也无法进行验证功能，所以需要使用TFTP简单文件传输协议来帮助客户端获取到引导及驱动文件。Vsftpd服务程序是用于传输完整系统安装镜像的，把安装镜像资料通过网络传输给客户机，当然只要把安装镜像传送过去就可以，因此同学们可以用Httpd来替代Vsftpd服务程序。

#### 部署相关服务程序
##### 配置DHCP服务程序
DHCP动态主机地址管理服务程序用于为客户主机网卡分配可用的IP地址，这是服务端与客户端主机进行文件传输的基础，因此第一个来配置吧。

```
yum install dhcp
```

咱们已经在前面第14章的课程中细致的学习过了DHCP服务程序的配置及部署方法，因此应该对配置参数还有些印象吧～忘记的话可以回去查一查。这次的配置文件与前面学习时的区别主要有两个，首先是允许了BOOTP引导程序协议，目的是让区域网内暂无系统的主机也能够获取到静态网卡IP地址，其次是在配置文件的最下面加载了叫做pxelinux.0的引导驱动文件，这样做的目的是让客户端主机网卡获取到IP地址后主动去获取引导驱动文件，自动进行下一步的安装过程。

```
vim /etc/dhcp/dhcpd.conf
allow booting;
allow bootp;
ddns-update-style interim;
ignore client-updates;
subnet 192.168.10.0 netmask 255.255.255.0 {
        option subnet-mask      255.255.255.0;
        option domain-name-servers  192.168.10.10;
        range dynamic-bootp 192.168.10.100 192.168.10.200;
        default-lease-time      21600;
        max-lease-time          43200;
        next-server             192.168.10.10;
        filename                "pxelinux.0";
}
```

确认DHCP服务程序参数填写正确后就可以重启一下服务，并把DHCP服务程序添加到开机启动项了～这样下一次重启服务器后依然能够自动化的为客户主机无人值守安装系统，一劳永逸。

```
systemctl restart dhcpd
systemctl enable dhcpd
```

##### 配置TFTP服务程序
咱们在第11章的课程中学习过vsftpd服务与TFTP服务，vsftpd是一款功能丰富的文件传输服务程序，需要用户使用匿名、本地甚至虚拟用户来进行访问验证，但当前的客户端主机连系统都没有安装，如何进行登录验证呢？而TFTP是一种基于UDP协议的简单文件传输协议，用户不需要进行验证即可获取到所需的文件资源，因此接下来配置TFTP服务程序来为用户提供引导及驱动文件吧～当客户端有了基本的驱动程序后再通过vsftpd服务程序把完整的光盘镜像文件传送过去。

```
yum install tftp-server
```

TFTP是一种非常精简的文件传输服务程序，它的运行和关闭时由xinted网络守护进程服务来进行统一管理的，平时xinetd服务程序会同时监听很多个系统的端口号，然后根据用户请求的端口号来调取对应的服务程序来响应用户的请求。如果咱们需要开启TFTP服务程序的话，就把xinetd服务程序对应的配置文件中disable参数改成no就可以了，意思是不要禁用TFTP服务程序，那也就是开启它了。配置文件修改好后保存退出，然后记得把xinetd服务程序重启一下，然后加入到开机启动项中（红帽RHEL7系统中xinetd服务程序默认已经启用，因此此时执行命令没有输出信息是正常情况）。

```
vim /etc/xinetd.d/tftp
service tftp
{
        socket_type             = dgram
        protocol                = udp
        wait                    = yes
        user                    = root
        server                  = /usr/sbin/in.tftpd
        server_args             = -s /var/lib/tftpboot
        disable                 = no
        per_source              = 11
        cps                     = 100 2
        flags                   = IPv4
```

```
systemctl restart xinetd
systemctl enable xinetd
```

TFTP服务程序默认会占用服务器udp协议的69端口号，所以在生产环境中还要记得在firewalld防火墙管理工具中写入一下永久生效的允许策略，让客户端主机能够顺利的获取到引导文件。

```
firewall-cmd --permanent --add-port=69/udp
firewall-cmd --reload 
```

##### 配置SYSLinux服务程序
SYSLinux是用于提供引导加载的服务程序，与其说SYSLinux说一个服务程序，不如说更需要里面的引导文件，安装好SYSLinux服务程序软件包后就会在/usr/share/syslinux目录中出现很多的引导文件。

```
yum install syslinux
```

咱们需要先把SYSLinux提供的引导文件复制到TFTP服务程序的默认目录中，这样用户就可以在无系统的情况下顺利的获取到引导文件了，刚刚提到的pxelinux.0文件就被放到这里啦（请同学们确认光盘镜像已被挂载到了/media/cdrom目录了）。

```
cd /var/lib/tftpboot
cp /usr/share/syslinux/pxelinux.0 .
cp /media/cdrom/images/pxeboot/{vmlinuz,initrd.img} .
cp /media/cdrom/isolinux/{vesamenu.c32,boot.msg} .
```

然后在TFTP服务程序的目录中新建一个叫做pxelinux.cfg的文件夹，虽然有后缀，但也依然是目录哦，不是一个文件！从系统光盘中把开机选项菜单复制到这里目录中命名为default，这个文件就是开机时候的选项菜单。

```
mkdir pxelinux.cfg
cp /media/cdrom/isolinux/isolinux.cfg pxelinux.cfg/default
```

默认的开机菜单中有两个选项，要么安装系统、要么对安装介质进行检验，而既然已经确定是要无人值守安装系统啦，如果每台主机都要手动选择一下的话也太麻烦啦。咱们就来编辑这个文件把第1行的default参数修改为linux，这样的话系统在开机时就会默认执行那个名称为Linux的选项啦，而对应的Linux选项在64行左右，把默认的光盘镜像安装方式修改成FTP网络文件传输方式，指定好光盘镜像的获取网址以及ks应答文件的获取路径

```
vim pxelinux.cfg/default
1 default linux
 2 timeout 600
 3
 4 display boot.msg
 5
 6 # Clear the screen when exiting the menu, instead of leaving the menu displa yed.
 7 # For vesamenu, this means the graphical background is still displayed witho ut
 8 # the menu itself for as long as the screen remains in graphics mode.
 9 menu clear
 10 menu background splash.png
 11 menu title Red Hat Enterprise Linux 7.0
 12 menu vshift 8
 13 menu rows 18
 14 menu margin 8
 15 #menu hidden
 16 menu helpmsgrow 15
 17 menu tabmsgrow 13
 18
 19 # Border Area
 20 menu color border * #00000000 #00000000 none
 21
 22 # Selected item
 23 menu color sel 0 #ffffffff #00000000 none
 24
 25 # Title bar
 26 menu color title 0 #ff7ba3d0 #00000000 none
 27
 28 # Press [Tab] message
 29 menu color tabmsg 0 #ff3a6496 #00000000 none
 30
 31 # Unselected menu item
 32 menu color unsel 0 #84b8ffff #00000000 none
 33
 34 # Selected hotkey
 35 menu color hotsel 0 #84b8ffff #00000000 none
 36
 37 # Unselected hotkey
 38 menu color hotkey 0 #ffffffff #00000000 none
 39
 40 # Help text
 41 menu color help 0 #ffffffff #00000000 none
 42 
 43 # A scrollbar of some type? Not sure.
 44 menu color scrollbar 0 #ffffffff #ff355594 none
 45 
 46 # Timeout msg
 47 menu color timeout 0 #ffffffff #00000000 none
 48 menu color timeout_msg 0 #ffffffff #00000000 none
 49 
 50 # Command prompt text
 51 menu color cmdmark 0 #84b8ffff #00000000 none
 52 menu color cmdline 0 #ffffffff #00000000 none
 53 
 54 # Do not display the actual menu unless the user presses a key. All that is displayed is a timeout message.
 55 
 56 menu tabmsg Press Tab for full configuration options on menu items.
 57 
 58 menu separator # insert an empty line
 59 menu separator # insert an empty line
 59 menu separator # insert an empty line
 60 
 61 label linux
 62 menu label ^Install Red Hat Enterprise Linux 7.0
 63 kernel vmlinuz
 64 append initrd=initrd.img inst.stage2=ftp://192.168.10.10 ks=ftp://192.168.10.10/pub/ks.cfg quiet
```

##### 配置VSFtpd服务程序
咱们这套无人值守安装系统服务的光盘镜像通过FTP协议进行传输，因此肯定少不了要用到vsftpd服务程序，当然只要能够把光盘镜像顺利的传送给客户端主机就达到目的啦，因此如果愿意的话也可以用httpd服务程序来提供HTTP网站访问方式，但是如果真的要用HTTP网站服务来提供光盘镜像，同学们可一定要记得把刚刚配置文件中的光盘镜像获取网址和ks应答文件获取网址修改一下哦～

```
yum install vsftpd
```

同学们可千万不要嫌弃刘遄老师啰嗦，配置文件安装配置后一定要添加到开机启动项中，这样在红帽认证考试或生产环境中才能够在服务器重启后依然为用户提供相应的服务，希望同学们读完手中这本《linux就该这么学》书籍后都能养成这个好习惯。

```
systemctl restart vsftpd
systemctl enable vsftpd
```

首先需要自行确认下系统光盘镜像是否已经被正常挂载到了/media/cdrom目录上了，然后把目录中的光盘镜像资料全部的复制到FTP服务工作目录中来。

```
cp -r /media/cdrom/* /var/ftp
```

复制光盘镜像全部的数据大致需要3-5分钟，这期间也不要闲着啦，动手用firewalld防火墙管理工具添加上对FTP协议的永久允许策略，然后在SELinux中开启对FTP传输的允许策略：

```
firewall-cmd --permanent --add-service=ftp
firewall-cmd --reload 

setsebool -P ftpd_connect_all_unreserved=on
```

##### 创建KickStart应答文件
咱们使用PXE+Kickstart部署的是一套无人值守安装系统服务，而不是单纯的无人值守传输系统光盘镜像服务，因此需要让客户端主机能够一边获取光盘镜像资料，一边还能够把安装过程中出现的选项帮咱们填写好。简单来说，如果您在生产环境中有一百台服务器要安装成相同的系统环境，那么在安装过程中点击的按钮和填写的信息也应该都是相同的，那么为何不创建出一个备忘录似的需求清单呢？让系统无人值守自动化安装的时候可以遇到选项便从该文件中找到对应的值，而不用每个信息都要人工输入啦~真正做到解放双手的无人值守自动化部署系统。

因此经过上面对无人值守安装系统的介绍，应该已经能猜到Kickstart其实并不能算是一个服务程序，而是应答文件了吧~是的！Kickstart应答文件中包含了系统安装过程中需要使用的选项和参数信息，系统通过调取这个应答文件的内容而最终实现了无人值守安装系统。那么既然这个文件如此重要，那么那里去找呢？其实在超级管理员root用户的家目录中有个叫做anaconda-ks.cfg的文件就是应答文件啦，咱们先把这个文件复制到vsftpd服务程序的工作目录中（在开机选项菜单配置文件中已经定义了该文件的获取路径，因此必须存放在pub目录中来）。最后记得使用chmod命令设置所有人都有可读取的权限，保证客户端主机可以顺利的获取到应答文件及里面的内容：

```
cp ~/anaconda-ks.cfg /var/ftp/pub/ks.cfg
chmod +r /var/ftp/pub/ks.cfg
```

Kickstart应答文件并没有想象中的那么复杂，总共只有46行左右的参数和注释内容，同学们可以通过参数的名称及介绍快速的了解每个参数的作用，因此刘遄老师就挑选几个比较有代表性的来修改下吧，其他参数可以自己动手修改试一试哦。首先把约第6行左右的光盘镜像安装方式修改成FTP远程文件协议，并仔细的填写好服务端地址，然后把约第21行左右的时区修改成亚洲/上海吧~最后再来把约29行左右的磁盘选项设置为清空所有磁盘内容并初始化

```
vim /var/ftp/pub/ks.cfg 
 1 #version=RHEL7
 2 # System authorization information
 3 auth --enableshadow --passalgo=sha512
 4 
 5 # Use CDROM installation media
 6 url --url=ftp://192.168.10.10
 7 # Run the Setup Agent on first boot
 8 firstboot --enable
 9 ignoredisk --only-use=sda
 10 # Keyboard layouts
 11 keyboard --vckeymap=us --xlayouts='us'
 12 # System language
 13 lang en_US.UTF-8
 14 
 15 # Network information
 16 network --bootproto=dhcp --device=eno16777728 --onboot=off --ipv6=auto
 17 network --hostname=localhost.localdomain
 18 # Root password
 19 rootpw --iscrypted $6$pDjJf42g8C6pL069$iI.PX/yFaqpo0ENw2pa7MomkjLyoae2zjMz2UZJ7b H3UO4oWtR1.Wk/hxZ3XIGmzGJPcs/MgpYssoi8hPCt8b/
 20 # System timezone
 21 timezone Asia/Shanghai --isUtc
 22 user --name=linuxprobe --password=$6$a9v3InSTNbweIR7D$JegfYWbCdoOokj9sodEccdO.zL F4oSH2AZ2ss2R05B6Lz2A0v2K.RjwsBALL2FeKQVgf640oa/tok6J.7GUtO/ --iscrypted --gecos ="linuxprobe"
 23 # X Window System configuration information
 24 xconfig --startxonboot
 25 # System bootloader configuration
 26 bootloader --location=mbr --boot-drive=sda
 27 autopart --type=lvm
 28 # Partition clearing information
 29 clearpart --all --initlabel
 30 
 31 %packages
 32 @base
 33 @core
 34 @desktop-debugging
 35 @dial-up
 36 @fonts
 37 @gnome-desktop
 38 @guest-agents
 39 @guest-desktop-agents
 40 @input-methods
 41 @internet-browser
 42 @multimedia
 43 @print-client
 44 @x11
 45 
 46 %end
```

如果认为系统默认自带的应答文件参数较少，或者不能够满足生产环境的需求，可以通过yum仓库来安装system-config-kickstart软件包，这是一款图形化的Kickstart应答文件生成工具，可以根据自己的需求定制出应答文件来，然后存放到/var/ftp/pub目录中并改成ks.cfg就可以啦。

##### 自动部署客户机
### 使用LNMP架构部署动态网站环境
#### LNMP动态网站架构
LNMP动态网站部署架构是一套由Linux系统+Nginx网站服务+Mysql数据库管理系统+PHP脚本语言组成的动态网站系统解决方案，LNMP中的L是Linux系统的意思，不仅可以是RHEL、Centos、Fedora，还可以是Debian、Ubuntu等等系统。

使用源码包安装服务程序前首先要让服务器具备编译程序源码的环境，其中包括C语言、C++语言、Perl语言的编译器，以及各种常见的编译支持函数库程序。

```
um install -y apr* autoconf automake bison bzip2 bzip2* compat* cpp curl curl-devel fontconfig fontconfig-devel freetype freetype* freetype-devel gcc gcc-c++ gd gettext gettext-devel glibc kernel kernel-headers keyutils keyutils-libs-devel krb5-devel libcom_err-devel libpng libpng-devel libjpeg* libsepol-devel libselinux-devel libstdc++-devel libtool* libgomp libxml2 libxml2-devel libXpm* libtiff libtiff* make mpfr ncurses* ntp openssl openssl-devel patch pcre-devel perl php-common php-gd policycoreutils telnet t1lib t1lib* nasm nasm* wget zlib-devel
```

刘遄老师已经把安装LNMP动态网站部署架构所需的16个软件源码包和1个用于检查效果的论坛网站系统软件包上传至了咱们书籍的下载服务器中，同学们可以在Windows系统中下载后通过ssh服务传送至服务器中，也可以直接在Linux服务器中使用wget命令下载到这些源码包文件，根据第六章学习到的FHS协议，建议把个人要安装的软件包存放在/usr/local/src目录中：

```
cd /usr/local/src

wget http://down.linuxprobe.com/Tools/cmake-2.8.11.2.tar.gz
wget http://down.linuxprobe.com/Tools/Discuz_X3.2_SC_GBK.zip
wget http://down.linuxprobe.com/Tools/freetype-2.5.3.tar.gz
wget http://down.linuxprobe.com/Tools/jpegsrc.v9a.tar.gz
wget http://down.linuxprobe.com/Tools/libgd-2.1.0.tar.gz
wget http://down.linuxprobe.com/Tools/libmcrypt-2.5.8.tar.gz
wget http://down.linuxprobe.com/Tools/libpng-1.6.12.tar.gz
wget http://down.linuxprobe.com/Tools/libvpx-v1.3.0.tar.bz2
wget http://down.linuxprobe.com/Tools/mysql-5.6.19.tar.gz
wget http://down.linuxprobe.com/Tools/nginx-1.6.0.tar.gz
wget http://down.linuxprobe.com/Tools/openssl-1.0.1h.tar.gz
wget http://down.linuxprobe.com/Tools/php-5.5.14.tar.gz
wget http://down.linuxprobe.com/Tools/pcre-8.35.tar.gz
wget http://down.linuxprobe.com/Tools/t1lib-5.1.2.tar.gz
wget http://down.linuxprobe.com/Tools/tiff-4.0.3.tar.gz
wget http://down.linuxprobe.com/Tools/yasm-1.2.0.tar.gz
wget http://down.linuxprobe.com/Tools/zlib-1.2.8.tar.gz

ls
zlib-1.2.8.tar.gz       libmcrypt-2.5.8.tar.gz  pcre-8.35.tar.gz
cmake-2.8.11.2.tar.gz   libpng-1.6.12.tar.gz    php-5.5.14.tar.gz
Discuz_X3.2_SC_GBK.zip  libvpx-v1.3.0.tar.bz2   t1lib-5.1.2.tar.gz
freetype-2.5.3.tar.gz   mysql-5.6.19.tar.gz     tiff-4.0.3.tar.gz
jpegsrc.v9a.tar.gz      nginx-1.6.0.tar.gz      yasm-1.2.0.tar.gz
libgd-2.1.0.tar.gz      openssl-1.0.1h.tar.gz
```

CMake是一款Linux系统中的常用编译工具，要想通过源码包安装服务程序就一定要严格遵守上面总结的安装步骤——下载解压、编译代码、生成二进制文件、运行安装程序。在接下来解压、编译各个软件包源码程序的时候都会有大量的输出信息，刘遄老师就会默认省略不写了，请读者以实际操作为准。

```
tar xzvf cmake-2.8.11.2.tar.gz
cd cmake-2.8.11.2/
./configure
make 
make install
```

#### 配置Mysql服务
在前面章节使用Yum软件仓库来安装服务程序时，系统会自动根据RPM软件包中的指令集完成配置软件信息等等工作，但一旦选择了用源码包的方式来安装服务程序，这一切就都需要自己手工完成了。例如需要先在系统中创建一个名为mysql的用户，他是专门用于负责运行Mysql数据库的用户，请记得把这类帐户的Bash终端都顺手设置成nologin，避免黑客通过该用户登陆到服务器中，提高系统安全性。

```
useradd mysql -s /sbin/nologin
```

创建一个用于保存Mysql数据库程序和数据库文件的目录，并把该目录的所有者和所有组身份修改为mysql，其中/usr/local/mysql是用于保存mysql数据库服务程序的目录，而/usr/local/mysql/var则是用于保存真实数据库文件的目录。

```
mkdir -p /usr/local/mysql/var
chown -Rf mysql:mysql /usr/local/mysql
```

接下来解压、编译、安装Mysql数据库服务程序，咱们在编译Mysql数据库的时候使用的是cmake命令，-DCMAKE_INSTALL_PREFIX参数用于定义Mysql数据库服务程序的保存目录，-DMYSQL_DATADIR参数用于定义真实数据库文件的目录，而-DSYSCONFDIR则是定义Mysql数据库配置文件所保存的目录，由于Mysql数据库服务程序的体积比较大，因此编译的过程是比较漫长的，在此期间可以稍微休息一下啦。

```
tar xzvf mysql-5.6.19.tar.gz
cd mysql-5.6.19/

cmake . -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/usr/local/mysql/var -DSYSCONFDIR=/etc

make
make install
```

为了让Mysql数据库程序正常的运转起来，需要先删除掉在/etc目录中的默认配置文件，然后在Mysql数据库程序目录的scripts文件夹内可以找到一个名为mysql_install_db的脚本程序，使用这条命令的--user参数指定mysql服务的对应帐号名称（在前面步骤已经创建），--basedir参数指定mysql服务程序的保存目录，以及用--datadir参数指定mysql真实数据库的文件保存目录，这样即可生成出系统数据库文件啦，也会生成出新的mysql服务配置文件。

```
rm -rf /etc/my.cnf
cd /usr/local/mysql
./scripts/mysql_install_db --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/var
```

把系统新生成的Mysql数据库配置文件做一个链接文件到/etc目录中，然后把程序目录中的开机程序文件复制到/etc/rc.d/init.d目录中，这样做的目的是为了让Mysql数据库服务程序能够通过service命令来进行管理啦，记得把权限修改成755以便于让用户有执行该脚本的权限：

```
ln -s my.cnf /etc/my.cnf 
cp ./support-files/mysql.server /etc/rc.d/init.d/mysqld
chmod 755 /etc/rc.d/init.d/mysqld
```

编辑刚刚复制来的Mysql数据库脚本文件，把大约46、47行的basedir与datadir参数分别修改为Mysql数据库程序保存目录和真实数据库文件内容。

```
vim /etc/rc.d/init.d/mysqld
 46 basedir=/usr/local/mysql
 47 datadir=/usr/local/mysql/var
```

配置好脚本文件后便可以用service命令启动mysqld数据库服务了，mysqld是Mysql数据库程序的服务名称，留意不要写错就可以，顺手再用chkconfig命令把mysqld服务程序加入到开机启动项中吧。

```
service mysqld start
chkconfig mysqld on
```

Mysql数据库程序有许多自带的命令，可是BASH终端的PATH变量并不会包含这些命令所存放的目录，因此咱们也就不能顺利的对Mysql数据库进行初始化，也不能使用Mysql数据库自带的命令啦。想要把命令所保存的目录永久性的定义到PATH变量中，需要编辑/etc/profile文件并写入追加的命令目录，这样当下一次服务器重启时就会永久生效了，如果不想重启就生效的话可以用source命令加载一下文件，那么新的PATH变量内容也就是立即生效了。

```
vim /etc/profile
 65 for i in /etc/profile.d/*.sh ; do
 66 if [ -r "$i" ]; then
 67 if [ "${-#*i}" != "$-" ]; then
 68 . "$i"
 69 else
 70 . "$i" >/dev/null
 71 fi
 72 fi
 73 done
 74 export PATH=$PATH:/usr/local/mysql/bin
 75 unset i
 76 unset -f pathmunge

source /etc/profile
```

Mysql数据库服务程序还有一些需要调用的程序文件和函数库文件，由于当前是通过源码包方式安装的Mysql数据库，因此现在也必须要手动的把这些文件链接过来了。

```
mkdir /var/lib/mysql
ln -s /usr/local/mysql/lib/mysql /usr/lib/mysql
ln -s /tmp/mysql.sock /var/lib/mysql/mysql.sock
ln -s /usr/local/mysql/include/mysql /usr/include/mysql
```

Mysql数据库服务程序已经启动，各个调用的函数文件也已经就位，PATH环境变量中也已经加入了mysql数据库命令的目录，这一切配置妥当后就可以对mysql数据库进行初始化了，初始化配置的过程与MariaDB数据库是一样的，是不是感觉很亲切呢，只是最后变成了Thanks for using MySQL!

```
mysql_secure_installation
```

#### 配置Nginx服务
Nginx是一款相当优秀的用于部署动态网站的服务程序，它最初是为俄罗斯门户站点而设计的网站服务软件，而作为一款轻量级的网站服务软件，因其稳定性和丰富的功能而深受信赖。其中最最最被认可的是低系统资源、占用内存少且并发能力强，目前国内如新浪、网易、腾讯等门户站点均已在使用，市场占有份额已达到20%左右（2017年最新数据），并且在不断的增长。

Nginx程序的稳定性来自于它采用了分阶段的资源分配技术，使得CPU与内存占用率会非常低，所以使用Nginx程序部署动态网站环境不仅十分的稳定、高效，而且消耗更少的系统资源。Nginx丰富的模块功能也几乎与Apache程序数量相同，现在已经完全的支持了proxy、rewrite、mod_fcgi、ssl、vhosts等常用模块。而且还支持了热部署技术，即能够可以7*24不间断提供服务，即便运行数月也无须重启，还可以在不暂停服务的情况下直接对Nginx服务程序进行升级。

坦白来讲，虽然Nginx程序的代码质量非常高，代码很规范，技术成熟，模块扩展也很容易，但Nginx依然存在不少问题，比如Nginx是由俄罗斯人创建的，所以在资料文档方面还并不完善，中文教材的质量更是鱼龙混杂。但Nginx服务程序近年来迅猛的增长势头，刘遄老师预测未来应该能够在轻量级HTTP服务器市场有不错的未来。
在正式安装Nginx服务程序前，咱们还需要为其解决掉相关的软件依赖关系，例如用于提供Perl语言兼容的正则表达式库的软件包pcre，快来解压、编译、生成、安装它的服务源码文件吧：

```
[root@linuxprobe ~]# cd /usr/local/src
[root@linuxprobe src]# tar xzvf pcre-8.35.tar.gz 
[root@linuxprobe src]# cd pcre-8.35
[root@linuxprobe pcre-8.35]# ./configure --prefix=/usr/local/pcre
[root@linuxprobe pcre-8.35]# make
[root@linuxprobe pcre-8.35]# make install 
```

openssl软件包是用于提供网站加密证书服务的程序文件，安装这些文件时需要自定义下服务程序的安装目录，以便于稍后调用它们的时候更可控。

```
[root@linuxprobe pcre-8.35]# cd /usr/local/src
[root@linuxprobe src]# tar xzvf openssl-1.0.1h.tar.gz
[root@linuxprobe src]# cd openssl-1.0.1h
[root@linuxprobe openssl-1.0.1h]# ./config --prefix=/usr/local/openssl
[root@linuxprobe openssl-1.0.1h]# make
[root@linuxprobe openssl-1.0.1h]# make install 
```

openssl软件包安装后默认会在/usr/local/openssl/bin目录中提供很多的可用命令，咱们需要像刚刚那样把这个目录添加到PATH环境变量中，并写入到配置文件里，最后执行一下source命令以便让新的PATH环境变量内容可以立即生效：

```
vim /etc/profile
export PATH=$PATH:/usr/local/mysql/bin:/usr/local/openssl/bin
```

zlib软件包是用于提供压缩功能的函数库文件，其实对这些与Nginx服务程序有调用关系的服务程序同学们不需要深入了解，只要大致了解其作用就已经足够了：

```
[root@linuxprobe pcre-8.35]# cd /usr/local/src
[root@linuxprobe src]# tar xzvf zlib-1.2.8.tar.gz 
[root@linuxprobe src]# cd zlib-1.2.8
[root@linuxprobe zlib-1.2.8]# ./configure --prefix=/usr/local/zlib
[root@linuxprobe zlib-1.2.8]# make
[root@linuxprobe zlib-1.2.8]# make install
```

依赖关系的软件包安装部署好之后，创建一个用于执行nginx服务程序的用户吧，账户名称可以自定义，但一定要记住，下面需要调用：

```
[root@linuxprobe zlib-1.2.8]# cd ..
[root@linuxprobe src]# useradd www -s /sbin/nologin
```

编译Nginx服务程序命令需要设置的参数特别的多，其中--prefix参数用于定义nginx服务程序稍后安装到的位置，--user与--group参数用于指定执行nginx服务程序的用户名和用户组。使用参数调用openssl、zlib、pcre软件包时请写软件源码包的解压路径，而不是程序的安装路径：

```
[root@linuxprobe src]# tar xzvf nginx-1.6.0.tar.gz 
[root@linuxprobe src]# cd nginx-1.6.0/
[root@linuxprobe nginx-1.6.0]# ./configure --prefix=/usr/local/nginx --without-http_memcached_module --user=www --group=www --with-http_stub_status_module --with-http_ssl_module --with-http_gzip_static_module --with-openssl=/usr/local/src/openssl-1.0.1h --with-zlib=/usr/local/src/zlib-1.2.8 --with-pcre=/usr/local/src/pcre-8.35
[root@linuxprobe nginx-1.6.0]# make
[root@linuxprobe nginx-1.6.0]# make install
```

nginx服务程序要想启动以及加入到开机启动项中也需要有脚本文件，但很可惜默认nginx软件包安装后并没有为用户提供，因此刘遄老师给同学们准备了一份可用的启动脚本文件，在/etc/rc.d/init.d目录中创建脚本文件并直接复制下面的脚本内容即可（已经有了第4章SHELL脚本的学习经历，这个脚本应该是可以大致看懂的吧）。

```
vim /etc/rc.d/init.d/nginx
#!/bin/bash
# nginx - this script starts and stops the nginx daemon
# chkconfig: - 85 15
# description: Nginx is an HTTP(S) server, HTTP(S) reverse \
# proxy and IMAP/POP3 proxy server
# processname: nginx
# config: /etc/nginx/nginx.conf
# config: /usr/local/nginx/conf/nginx.conf
# pidfile: /usr/local/nginx/logs/nginx.pid
# Source function library.
. /etc/rc.d/init.d/functions
# Source networking configuration.
. /etc/sysconfig/network
# Check that networking is up.
[ "$NETWORKING" = "no" ] && exit 0
nginx="/usr/local/nginx/sbin/nginx"
prog=$(basename $nginx)
NGINX_CONF_FILE="/usr/local/nginx/conf/nginx.conf"
[ -f /etc/sysconfig/nginx ] && . /etc/sysconfig/nginx
lockfile=/var/lock/subsys/nginx
make_dirs() {
# make required directories
user=`$nginx -V 2>&1 | grep "configure arguments:" | sed 's/[^*]*--user=\([^ ]*\).*/\1/g' -`
        if [ -z "`grep $user /etc/passwd`" ]; then
                useradd -M -s /bin/nologin $user
        fi
options=`$nginx -V 2>&1 | grep 'configure arguments:'`
for opt in $options; do
        if [ `echo $opt | grep '.*-temp-path'` ]; then
                value=`echo $opt | cut -d "=" -f 2`
                if [ ! -d "$value" ]; then
                        # echo "creating" $value
                        mkdir -p $value && chown -R $user $value
                fi
        fi
done
}
start() {
[ -x $nginx ] || exit 5
[ -f $NGINX_CONF_FILE ] || exit 6
make_dirs
echo -n $"Starting $prog: "
daemon $nginx -c $NGINX_CONF_FILE
retval=$?
echo
[ $retval -eq 0 ] && touch $lockfile
return $retval
}
stop() {
echo -n $"Stopping $prog: "
killproc $prog -QUIT
retval=$?
echo
[ $retval -eq 0 ] && rm -f $lockfile
return $retval
}
restart() {
#configtest || return $?
stop
sleep 1
start
}
reload() {
#configtest || return $?
echo -n $"Reloading $prog: "
killproc $nginx -HUP
RETVAL=$?
echo
}
force_reload() {
restart
}
configtest() {
$nginx -t -c $NGINX_CONF_FILE
}
rh_status() {
status $prog
}
rh_status_q() {
rh_status >/dev/null 2>&1
}
case "$1" in
start)
        rh_status_q && exit 0
        $1
        ;;
stop)
        rh_status_q || exit 0
        $1
        ;;
restart|configtest)
$1
;;
reload)
        rh_status_q || exit 7
        $1
        ;;
force-reload)
        force_reload
        ;;
status)
        rh_status
        ;;
condrestart|try-restart)
        rh_status_q || exit 0
        ;;
*)
echo $"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"
exit 2
esac
```

保存脚本文件后记得给予755权限，以便能够来执行这个脚本哦～然后以路径的方式执行这个脚本并给予restart重启该服务的参数执行，并使用chkconfig命令将其添加至开机启动项中，大功告成啦。

```
[root@linuxprobe nginx-1.6.0]# chmod 755 /etc/rc.d/init.d/nginx
[root@linuxprobe nginx-1.6.0]# /etc/rc.d/init.d/nginx restart
Restarting nginx (via systemctl):                          [  OK  ]
[root@linuxprobe nginx-1.6.0]# chkconfig nginx on
```

Nginx服务程序启动后就可以在浏览器中输入服务器本地网卡IP地址查看到默认网页啦，相比于Apache服务程序的那种红色的默认页面，Nginx服务程序显得更加的简洁吧。

#### 配置php服务
PHP超文本预处理器是一种通用的开源脚本语言，起始于1995年，它吸取了如C语言、Java语言及Perl语言很多的优点，具有开源、免费、快捷、跨平台性强、效率高等等优良特性，是目前Web开发领域最常用的语言之一，咱们的《Linux就该这么学》书籍在线学习站点系统就是基于PHP语言环境编写及运行的。而使用源码编译安装PHP语言环境其实并不复杂，难点在于解决PHP语言程序包和其他软件的依赖关系上，需要先安装部署将近十个用于搭建网站页面的软件程序包，然后才能正式安装PHP语言程序。

yasm源码包是一款常见的开源汇编器，解压、编译、安装过程输出信息已省略：

```
[root@linuxprobe nginx-1.6.0]# cd ..
[root@linuxprobe src]# tar zxvf yasm-1.2.0.tar.gz
[root@linuxprobe src]# cd yasm-1.2.0
[root@linuxprobe yasm-1.2.0]# ./configure
[root@linuxprobe yasm-1.2.0]# make
[root@linuxprobe yasm-1.2.0]# make install
```

libmcrypt源码包是用于加密算法的扩展库程序，解压、编译、安装过程输出信息已省略：

```
[root@linuxprobe yasm-1.2.0]# cd ..
[root@linuxprobe src]# tar zxvf libmcrypt-2.5.8.tar.gz
[root@linuxprobe src]# cd libmcrypt-2.5.8
[root@linuxprobe libmcrypt-2.5.8]# ./configure
[root@linuxprobe libmcrypt-2.5.8]# make
[root@linuxprobe libmcrypt-2.5.8]# make install
```

libvpx源码包是用于提供视频编码器的服务程序，解压、编译、安装过程输出信息已省略，肯定有很多不细心的同学顺手就用了tar命令的xzvf参数，但仔细看一下就会发现libvpx源码包的后缀是.tar.bz2，也就是用bzip2格式进行的压缩，因此解压的正确参数应该是xjvf哦~：

```
[root@linuxprobe libmcrypt-2.5.8]# cd ..
[root@linuxprobe src]# tar xjvf libvpx-v1.3.0.tar.bz2
[root@linuxprobe src]# cd libvpx-v1.3.0
[root@linuxprobe libvpx-v1.3.0]# ./configure --prefix=/usr/local/libvpx --enable-shared --enable-vp9
[root@linuxprobe libvpx-v1.3.0]# make
[root@linuxprobe libvpx-v1.3.0]# make install
```

tiff源码包是用于提供标签图像文件格式的服务程序，解压、编译、安装过程输出信息已省略：

```
[root@linuxprobe libvpx-v1.3.0]# cd ..
[root@linuxprobe src]# tar zxvf tiff-4.0.3.tar.gz
[root@linuxprobe src]# cd tiff-4.0.3
[root@linuxprobe tiff-4.0.3]# ./configure --prefix=/usr/local/tiff --enable-shared
[root@linuxprobe tiff-4.0.3]# make
[root@linuxprobe tiff-4.0.3]# make install
```

libpng源码包是用于提供png图片格式支持函数库的服务程序，解压、编译、安装过程输出信息已省略：

```
[root@linuxprobe tiff-4.0.3]# cd ..
[root@linuxprobe src]# tar zxvf libpng-1.6.12.tar.gz
[root@linuxprobe src]# cd libpng-1.6.12
[root@linuxprobe libpng-1.6.12]# ./configure --prefix=/usr/local/libpng --enable-shared
[root@linuxprobe libpng-1.6.12]# make
[root@linuxprobe libpng-1.6.12]# make install
```

freetype源码包是用于提供字体支持引擎的服务程序，解压、编译、安装过程输出信息已省略：

```
[root@linuxprobe libpng-1.6.12]# cd ..
[root@linuxprobe src]# tar zxvf freetype-2.5.3.tar.gz
[root@linuxprobe src]# cd freetype-2.5.3
[root@linuxprobe freetype-2.5.3]# ./configure --prefix=/usr/local/freetype --enable-shared
[root@linuxprobe freetype-2.5.3]# make
[root@linuxprobe freetype-2.5.3]# make install
```

jpeg源码包是用于提供jpeg图片格式支持函数库的服务程序，解压、编译、安装过程输出信息已省略：

```
[root@linuxprobe freetype-2.5.3]# cd ..
[root@linuxprobe src]# tar zxvf jpegsrc.v9a.tar.gz
[root@linuxprobe src]# cd jpeg-9a
[root@linuxprobe jpeg-9a]# ./configure --prefix=/usr/local/jpeg --enable-shared
[root@linuxprobe jpeg-9a]# make
[root@linuxprobe jpeg-9a]# make install
```

libgd源码包是用于提供图形处理的服务程序，解压、编译、安装过程输出信息已省略，而在编译libgd源码包的时候请记得写入的是jpeg、libpng、freetype、tiff、libvpx等服务程序在系统中的安装路径，即在上面安装过程中使用--prefix参数指定的目录路径：

```
[root@linuxprobe jpeg-9a]# cd ..
[root@linuxprobe src]# tar zxvf libgd-2.1.0.tar.gz
[root@linuxprobe src]# cd libgd-2.1.0
[root@linuxprobe libgd-2.1.0]# ./configure --prefix=/usr/local/libgd --enable-shared --with-jpeg=/usr/local/jpeg --with-png=/usr/local/libpng --with-freetype=/usr/local/freetype --with-fontconfig=/usr/local/freetype --with-xpm=/usr/ --with-tiff=/usr/local/tiff --with-vpx=/usr/local/libvpx
[root@linuxprobe libgd-2.1.0]# make
[root@linuxprobe libgd-2.1.0]# make install
```

t1lib源码包是用于提供图片生成函数库的服务程序，解压、编译、安装过程输出信息已省略，安装后把/usr/lib64目录中的函数文件做一个连接到/usr/lib目录中，以便系统能够顺利调取到函数文件：

```
[root@linuxprobe cd libgd-2.1.0]# cd ..
[root@linuxprobe src]# tar zxvf t1lib-5.1.2.tar.gz
[root@linuxprobe src]# cd t1lib-5.1.2
[root@linuxprobe t1lib-5.1.2]# ./configure --prefix=/usr/local/t1lib --enable-shared
[root@linuxprobe t1lib-5.1.2]# make
[root@linuxprobe t1lib-5.1.2]# make install
[root@linuxprobe t1lib-5.1.2]# ln -s /usr/lib64/libltdl.so /usr/lib/libltdl.so 
[root@linuxprobe t1lib-5.1.2]# cp -frp /usr/lib64/libXpm.so* /usr/lib/
```

此时终于把编译php服务源码包的相关软件包都已经安装部署妥当了，在开始编译源码包前先定义一个名称为LD_LIBRARY_PATH的全局环境变量，该环境变量的作用是帮助系统找到指定的动态链接库文件，是编译php服务源码包的必须元素之一。编译php服务源码包时除了定义要安装到的目录以外，还需要依次定义配置Php服务配置文件保存目录、Mysql数据库服务程序所在目录、Mysql数据库服务程序配置文件所在目录以及libpng、jpeg、freetype、libvpx、zlib、t1lib等等服务程序的安装目录路径，并通过参数启动php服务程序的诸多默认功能：

```
[root@linuxprobe t1lib-5.1.2]# cd ..
[root@linuxprobe src]# tar -zvxf php-5.5.14.tar.gz
[root@linuxprobe src]# cd php-5.5.14
[root@linuxprobe php-5.5.14]# export LD_LIBRARY_PATH=/usr/local/libgd/lib
[root@linuxprobe php-5.5.14]# ./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --with-mysql=/usr/local/mysql --with-mysqli=/usr/local/mysql/bin/mysql_config --with-mysql-sock=/tmp/mysql.sock --with-pdo-mysql=/usr/local/mysql --with-gd --with-png-dir=/usr/local/libpng --with-jpeg-dir=/usr/local/jpeg --with-freetype-dir=/usr/local/freetype --with-xpm-dir=/usr/ --with-vpx-dir=/usr/local/libvpx/ --with-zlib-dir=/usr/local/zlib --with-t1lib=/usr/local/t1lib --with-iconv --enable-libxml --enable-xml --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --enable-opcache --enable-mbregex --enable-fpm --enable-mbstring --enable-ftp --enable-gd-native-ttf --with-openssl --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip --enable-soap --without-pear --with-gettext --enable-session --with-mcrypt --with-curl --enable-ctype 
[root@linuxprobe php-5.5.14]# make
[root@linuxprobe php-5.5.14]# make install
```

在等待php源码包程序安装完成后，需要删除掉当前默认的配置文件，然后从php服务程序目录中复制对应的配置文件过来：

```
[root@linuxprobe php-5.5.14]# rm -rf /etc/php.ini
[root@linuxprobe php-5.5.14]# ln -s /usr/local/php/etc/php.ini /etc/php.ini
[root@linuxprobe php-5.5.14]# cp php.ini-production /usr/local/php/etc/php.ini
[root@linuxprobe php-5.5.14]# cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
[root@linuxprobe php-5.5.14]# ln -s /usr/local/php/etc/php-fpm.conf /etc/php-fpm.conf
```

php-fpm.conf是php服务程序的重要配置文件之一，咱们需要将其配置内容中约25行左右的pid文件保存路径启用，并在约148-149行的user与group参数分别修改为www帐户和用户组名称：

```
[root@linuxprobe php-5.5.14]# vim /usr/local/php/etc/php-fpm.conf
1 ;;;;;;;;;;;;;;;;;;;;;
2 ; FPM Configuration ;
3 ;;;;;;;;;;;;;;;;;;;;;
4 
5 ; All relative paths in this configuration file are relative to PHP's instal l
6 ; prefix (/usr/local/php). This prefix can be dynamically changed by using t he
7 ; '-p' argument from the command line.
8 
9 ; Include one or more files. If glob(3) exists, it is used to include a bunc h of
10 ; files from a glob(3) pattern. This directive can be used everywhere in the
11 ; file.
12 ; Relative path can also be used. They will be prefixed by:
13 ; - the global prefix if it's been set (-p argument)
14 ; - /usr/local/php otherwise
15 ;include=etc/fpm.d/*.conf
16 
17 ;;;;;;;;;;;;;;;;;;
18 ; Global Options ;
19 ;;;;;;;;;;;;;;;;;;
20 
21 [global]
22 ; Pid file
23 ; Note: the default prefix is /usr/local/php/var
24 ; Default Value: none
25 pid = run/php-fpm.pid
26 
………………省略部分输出信息………………
145 ; Unix user/group of processes
146 ; Note: The user is mandatory. If the group is not set, the default user's g roup
147 ; will be used.
148 user = www
149 group = www
150 
………………省略部分输出信息………………
```

配置妥当后便可把服务管理脚本文件复制到/etc/rc.d/init.d中啦，为了能够有执行脚本请记得要给予755权限，最后把php-fpm服务程序加入到开机启动项中：

```
[root@linuxprobe php-5.5.14]# cp sapi/fpm/init.d.php-fpm /etc/rc.d/init.d/php-fpm
[root@linuxprobe php-5.5.14]# chmod 755 /etc/rc.d/init.d/php-fpm
[root@linuxprobe php-5.5.14]# chkconfig php-fpm on
```

由于php服务程序的配置参数直接会影响到Web网站服务的运行环境，如果默认开启了例如允许用户在网页中执行Linux命令等等不必要且高危的功能，进而会降低了骇客入侵网站的难度，甚至加大了骇客提权到整台服务器的管理权限的几率。因此需要编辑php.ini配置文件，在约305左右的disable_functions参数后面追加上要禁止的功能名称吧，下面的禁用功能名单是刘遄老师依据运营网站经验而定制的，也许并不能适合每个生产环境，同学们可以在此基础上根据自身工作要求而酌情删减：

```
[root@linuxprobe php-5.5.14]# vim /usr/local/php/etc/php.ini
………………省略部分输出信息………………
300 
301 ; This directive allows you to disable certain functions for security reasons.
302 ; It receives a comma-delimited list of function names. This directive is
303 ; *NOT* affected by whether Safe Mode is turned On or Off.
304 ; http://php.net/disable-functions
305 disable_functions = passthru,exec,system,chroot,scandir,chgrp,chown,shell_exec,proc_open,proc_get_status,ini_alter,ini_alter,ini_restor e,dl,openlog,syslog,readlink,symlink,popepassthru,stream_socket_server,escapeshellcmd,dll,popen,disk_free_space,checkdnsrr,checkdnsrr,g etservbyname,getservbyport,disk_total_space,posix_ctermid,posix_get_last_error,posix_getcwd,posix_getegid,posix_geteuid,posix_getgid,po six_getgrgid,posix_getgrnam,posix_getgroups,posix_getlogin,posix_getpgid,posix_getpgrp,posix_getpid,posix_getppid,posix_getpwnam,posix_ getpwuid,posix_getrlimit,posix_getsid,posix_getuid,posix_isatty,posix_kill,posix_mkfifo,posix_setegid,posix_seteuid,posix_setgid,posix_ setpgid,posix_setsid,posix_setuid,posix_strerror,posix_times,posix_ttyname,posix_uname
306 
………………省略部分输出信息………………
```

这样就把php服务程序配置妥当了，最后还需要再编辑一下nginx服务程序的主配置文件，把约第2行的#（井号）去除，后面写上负责运行nginx服务程序的帐户名称用户组名称，把约45行左右的index参数后面写上网站的首页名称。最后是把约第65-71行参数前的#（井号）去除来启动参数，主要是修改约第69行的脚本名称路径参数，其中$document_root变量即为网站资料存储的根目录路径，若没有设置的话nginx服务程序是找不到网站资料的，因此会提示出404页面未找到的报错信息，确认参数信息填写正确后便可重启nginx服务与php-fpm服务，结束了对LNMP动态网站环境架构的配置实验。

```
[root@linuxprobe php-5.5.14]# vim /usr/local/nginx/conf/nginx.conf
 1 
 2 user www www;
 3 worker_processes 1;
 4 
 5 #error_log logs/error.log;
 6 #error_log logs/error.log notice;
 7 #error_log logs/error.log info;
 8 
 9 #pid logs/nginx.pid;
 10 
 11 
………………省略部分输出信息………………
 40 
 41 #access_log logs/host.access.log main;
 42 
 43 location / {
 44 root html;
 45 index index.html index.htm index.php;
 46 }
 47 
………………省略部分输出信息………………
 62 
 63 #pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
 64 
 65 location ~ \.php$ {
 66 root html;
 67 fastcgi_pass 127.0.0.1:9000;
 68 fastcgi_index index.php;
 69 fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
 70 include fastcgi_params;
 71 }
 72 
………………省略部分输出信息………………
[root@linuxprobe php-5.5.14]# systemctl restart nginx
[root@linuxprobe php-5.5.14]# systemctl restart php-fpm
```

#### 搭建Discuz论坛
 [搭建Discuz论坛](http://www.linuxprobe.com/chapter-20.html#203_Discuz)
为了能够检验LNMP动态网站环境是否配置妥当，咱们可以使用Discuz系统部署上去试试看，Discuz X3.2是由国内最常见的社区论坛系统，经过十多年的研发过程已经成为了全球成熟度最高、覆盖率最广的论坛网站系统之一。如果能够在部署的LNMP动态网站环境中成功安装使用Discuz论坛系统，也就意味着这套架构是可用的。Discuz X3.2软件包的后缀是.zip格式，因此应当用专用的unzip命令来进行解压操作，解压后会在当前目录中出现一个名为upload的文件目录，这里面保存的就是Discuz论坛的资料程序，咱们把nginx服务程序网站根目录的内容清空后就可以把这些文件都复制进去了，记得把该目录的所有者和所有组修改为www用户，并设置755权限以便于能够读、写、执行该论坛系统内的文件。

```
[root@linuxprobe php-5.5.14 ]# cd /usr/local/src/
[root@linuxprobe src]# unzip Discuz_X3.2_SC_GBK.zip
[root@linuxprobe src]# rm -rf /usr/local/nginx/html/{index.html,50x.html}*
[root@linuxprobe src]# mv upload/* /usr/local/nginx/html/
[root@linuxprobe src]# chown -Rf www:www /usr/local/nginx/html
[root@linuxprobe src]# chmod -Rf 755 /usr/local/nginx/html
```

#### 选购服务器主机
人们日常访问的网站是由域名、网站源程序和主机空间共同组成的，其中主机空间则是用于存放网页源代码并能够把网页内容展示给用户的服务器，既然同学们已经结束了对《Linux就该这么学》基础篇的学习旅程，刘遄老师再最后啰嗦一下这几年做网站总结的服务器主机选购的技巧吧，这样您也可以去搭建出一个属于自己的博客或论坛网站啦，能够把自己所学的Linux技术干货分享给更多人，愿开源世界美好的脚步更快一些。

虚拟主机:在一台服务器中分出一定的磁盘空间供用户放置网站、存放数据等，仅提供基础的网站访问、数据存放与传输流量功能，能够极大的降低用户费用，也几乎不需要管理员维护除网站数据以外的服务，适合小型网站。

VPS(Virtual Private Srver):在一台服务器中利用OpenVZ、Xen或KVM等虚拟化技术模拟出多个“主机”，每个主机都有独立的IP地址、操作系统，实现不同VPS之间磁盘空间、内存、CPU资源、进程与系统配置间的完全隔离，管理员可自由使用分配到的主机中的所有资源，所以需要有一定的维护系统的能力，适合小型网站。

云服务器(ECS):是一种整合了计算、存储、网络，能够做到弹性伸缩的计算服务，其使用起来与VPS几乎一样，但差别是云服务器建立在一组集群服务器中，每个服务器都会保存一个主机的镜像（备份），大大的提升了安全稳定性，另外还具备了灵活性与扩展性，用户只需按使用量付费即可，适合大中小型网站。

独立服务器:这台服务器仅提供给您使用，详细来讲又可以区分为租用方式与托管方式。租用方式是用户只需把硬件配置要求告知IDC服务商，服务器硬件设备由机房负责维护，运维管理员一般需要自行安装相应的软件并部署网站服务，租期可以为月、季、年，减轻了用户初期对硬件设备的投入，适合大中型网站。托管方式是用户需要自行购置服务器后交给IDC服务供应商的机房进行管理(缴纳管理服务费用)，用户对服务器硬件配置有完全的控制权，自主性强，但需要自行维护、修理服务器硬件设备，适合大中型网站。

另外有必要提醒读者，选择主机空间供应商时请一定要注意看口碑，综合分析再决定购买，某些供应商会有限制功能、强制添加广告、隐藏扣费或强制扣费等恶劣行为，一定一定不要上当!

## 课外
### [使用Git分布式版本控制系统](http://www.linuxprobe.com/chapter-21.html)
#### 分布式版本控制系统
我想大家还记得前面章节中谈到过Linus torvalds在1991年时发布了Linux操作系统吧，从那以后Linux系统便不断发展壮大，因为Linux系统开源的特性，所以一直接受着来自全球Linux技术爱好者的贡献，志愿者们通过邮件向Linus发送着自己编写的源代码文件，然后由Linus本人通过手工的方式将代码合并，但这样不仅没有效率，而且真的是太痛苦了。
一直到2002年，Linux系统经过十余年的不断发展，代码库已经庞大到无法再通过手工的方式管理了，但是Linus真的很不喜欢类似于CVS或者Subversion的一些版本控制系统，于是商业公司BitMover决定将其公司的BitKeeper分布式版本控制系统授权给Linux开发社区来免费使用，当时的BitKeeper可以比较文件内容的不同，还能够将出错的文档还原到历史某个状态，Linus终于放下了心里的石头。

CVS和Subversion属于传统的版本控制系统，而分布式版本控制系统最大的特点是不需要每次提交都把文件推送到版本控制服务器，而是采用分布式版本库的机制，使得每个开发人员都够从服务器中克隆一份完整的版本库到自己计算机本地，不必再完全依赖于版本控制服务器，使得源代码的发布和合并更加方便，并且因为数据都在自己本地，不仅效率提高了，而且即便我们离开了网络依然可以执行提交文件、查看历史版本记录、创建分支等等操作，真的是开发者的福音啊。

就这样平静的度过了三年时间，但是Linux社区聚集着太多的黑客人物，2005年时，那位曾经开发Samba服务程序的Andrew因为试图破解BitKeeper软件协议而激怒了BitMover公司，当即决定不再向Linux社区提供免费的软件授权了，此时的Linus其实也早已有自己编写分布式版本控制系统的打算了，于是便用C语言创建了Git分布式版本控制系统，并上传了Linux系统的源代码。

Git不仅是一款开源的分布式版本控制系统，而且有其独特的功能特性，例如大多数的分布式版本控制系统只会记录每次文件的变化，说白了就是只会关心文件的内容变化差异，而Git则是关注于文件数据整体的变化，直接会将文件提交时的数据保存成快照，而非仅记录差异内容，并且使用SHA-1加密算法保证数据的完整性。

>Git为了提高效率，对于没有被修改的文件，则不会重复存储，而是创建一个链接指向之前存储过的文件。


### [使用openstack部署云计算服务环境](http://www.linuxprobe.com/chapter-22.html)
#### 了解云计算

人类基于千年的物种衍变基础，在这个世纪终于有了爆发式的科技成果，尤其这二十年内互联网的发展，更像是一种催化剂，让原本已经热闹的地球更加的沸腾，互联网经济泡沫破灭后的科技研发却变得更加卖力，一次次的突破着传统研究中对人类脑力、科技最终式的定义，把“来自未来”的产品带到用户面前，那么到底互联网未来会变成什么样子，人类最终的归宿会是怎么样，我们不得而知，但可以肯定的是科技研发一直是由人类需求来驱动的。

众所周知Google谷歌是一家致力于互联网搜索、云计算、广告技术等领域的科技企业，一直在努力为全球无数的用户提供着大量基于互联网的产品与服务，而Amazon亚马逊则是全美国最大的网络电子商务公司，销售内容涉及方方面面，业务范围更是遍布全球，对于这种互联网巨头企业自然少不了庞大的基础设施的支撑，但是传统的硬件设施一旦投入就要一大笔钱，并且在业务的淡季也要一直的空闲，这样无疑产生了资源和资金的巨大浪费，所以最初的云计算便是由Google与Amazon分别提出的，核心理念之一就是通过云计算服务降低用户对资源拥有的成本。

当用户能够通过互联网方便的获取到计算、存储等服务时，我们比喻自己使用到了“云计算”，云计算并不能被称为是一种计算技术，而更像是一种服务模式，云计算服务好像拥有无穷的力量，能够预测气候变化、还能够模拟核弹爆炸，好像只要你需要，“云”就可以为你提供每秒万亿次的计算服务，满足你的一切需求，每个运维人员心里都有一个对云计算的理解，而最普遍接受的是NIST(美国国家标准与技术研究院)的定义：

>云计算是一种按使用量付费的服务模式，这是一种能够提供可用的、便捷的、按需求的网络访问模式，计算共享池能够快速的为用户提供网络、服务器、存储、应用软件及其他服务，并且只需要花费很少的管理时间。

NIST还针对于云计算的服务模式提出了3个服务层次：

>Iaas：提供给用户的是云计算基础设施，包括CPU、内存、存储、网络等其他的资源服务，用户不需要控制存储与网络等基础设施。
>Paas：提供给用户的是云计算中的开发和分发应用的解决方案，用户能够部署应用程序，也可以控制相关的托管环境，比如云服务器及操作系统，但用户不需要接触到云计算中的基础设施。
>Saas：提供给用户的是云计算基础设施上的应用程序，用户只需要在客户端界面访问即可使用到所需资源，而接触不到云计算的基础设施。

#### Openstack项目
Openstack最初是由NASA和Rackspace共同发起的云端计算服务项目，该项目以Apache许可证授权的方式成为了一款开源产品，目的是将多个组件整合后从而实现一个开源的云计算平台，目前Openstack项目正在被红帽、IBM、AMD、Intel、戴尔、思科、微软等超过一百家厂商共同研发，并已经支持了几乎所有的常见云计算环境，拥有了良好的可扩展性，而且部署搭建Openstack服务也变得十分简单，目前国内对于云计算的需求也逐渐增加，华胜天成、高德地图、京东、阿里巴巴、百度、中兴、华为等中国企业也加入到了Openstack项目研发当中，Openstack项目也正在随着全球内得到了众多厂商的参与支持而快速成熟。

Open是开放，Stack则是堆砌之意，合起来就是将众多的功能服务堆积起来的集合，让人们通过Openstack云计算项目，能够将诸如计算能力、存储、网络和软件等资源抽象成服务，以便让用户可以通过互联网远程来享用，付费的形式也变得因需而定，调整方便，拥有极强的虚拟可扩展性，是公共和私有云的建设与管理软件中的优秀开源项目。

Openstack作为一个云平台的管理项目，其功能组件覆盖了网络、虚拟化、操作系统、服务器等多个方面，每个功能组件交由不同的项目委员会来研发和管理，目前核心的项目包括有：

|功能	项目名称	描述|
|:|
|计算服务|Nova|负责虚拟机的创建、开关机、挂起、迁移、调整CPU、内存等规则。|
|对象存储|Swift|用于在大规模可扩展系统中通过内置的冗余及高容差机制实现对象存储的系统。|
|镜像服务|Glance|用于创建、上传、删除、编辑镜像信息的虚拟机镜像查找及索引系统。|
|身份服务|Keystone|为其他的功能服务提供身份验证、服务规则及服务令牌的功能。|
|网络管理|Neutron|用于为其他服务提供云计算的网络虚拟化技术，可自定义各种网络规则，支持主流的网络厂商技术。|
|块存储|Cinder|为虚拟机实例提供稳定的数据块存储的创建、删除、挂载、卸载、管理等服务。|
|图形界面|Horizon|为用户提供简单易用的Web管理界面，降低用户对功能服务的操作难度。|
|测量服务|Ceilometer|收集项目内所有的事件，用于监控、计费或为其他服务提供数据支撑。|
|部署编排|Heat|实现通过模板方式进行自动化的资源环境部署服务。|
|数据库服务|Trove|为用户提供可扩展的关系或非关系性数据库服务。|

Openstack项目的版本按照ABCDEFG……的顺序发布，每6个月更新一次，Openstack版本发布历史：

|版本名称|发布时间|
|:|
|Liberty|2015年10月15日|
|Kilo|2015年4月30日|
|Juno|2014年10月16日|
|Icehouse|2014年4月17日|
|Havana|2013年10月17日|
|Grizzly|2014年4月4日|
|Folsom|2012年9月27日|
|Essex|2012年4月5日|
|Diablo|2011年9月22日|
|Cactus|2011年4月15日|
|Bexar|2011年2月3日|
|Austin|2010年10月21日|

开源社区成员和Linux技术爱好者可以选择使用Openstack RDO版本，RDO版本允许用户以免费授权的方式来获取openstack软件的使用资格，但是从安装开始便较为复杂（需要自行解决诸多的软件依赖关系），而且没有官方给予的保障及售后服务，请读者们仔细的按实验步骤安装，就一定没有问题的~

#### 服务模块组件详解
Openstack是一个云计算的平台，也像是部署云操作系统的工具集，可以通过调取不同的组件来构建虚拟计算及云计算服务，比较重要的包括有计算（compute）、对象存储（Objectstorage）、认证（Identity）、仪表板（Dashboard）、块存储（Block Storage）、网络（Network）和镜像服务（image service）。

Nova提供计算服务

Nova可以称作是Openstack云计算平台中最核心的服务组件了，它作为计算的弹性控制器来管理虚拟化、网络及存储等资源，为Openstack的云主机实例提供可靠的支撑，其功能由不同的API来提供。

Nova-api(API服务器):
>API服务器用于提供云计算设施与外界交互的接口，也是用户对云计算设施进行管理的唯一通道，用户通过网页来调用各种API接口，再由API服务器通过消息队列把请求传递至目标设置进行处理。

Rabbit MQ Server(消息队列):
>Openstack在遵循AMQP高级消息队列协议的基础之上采用了消息队列进行通信，异步通信的方式更是能够减少了用户的等待时间，让整个平台都变得更有效率。

Nova-compute(运算工作站):
>运算工作站通过消息队列接收用户的请求并执行，从而负责对主机实例的整个生命周期中的各种操作进行处理，一般会架设多台计算工作站，根据调度算法来按照实例在任意一个计算工作站上部署。

Nova-network(网络控制器):
>用于处理主机的网络配置，例如分配IP地址，配置项目VLAN，设定安全群组及为计算节点配置网络。

Nova-Volume(卷工作站):
>基于LVM的实例卷能够为一个主机实例创建、删除、附加卷或从主机中分离卷。

Nova-scheduler(调度器)
>调度器以名为"nova-schedule"的守护进程方式进行运行，根据对比CPU架构及负载、内存占用率、子节点的远近等因素，使用调度算法从可用的资源池中选择运算服务器。

Glance提供镜像服务
>Openstack镜像服务是一套用于主机实例来发现、注册、索引的系统，功能相比较也很简单，具有基于组件的架构、高可用、容错性、开发标准等优良特性，虚拟机的镜像可以被放置到多种存储上。

Swift提供存储服务
>Swift模块是一种分布式、持续虚拟对象存储，具有跨节点百级对象的存储能力，并且支持内建冗余和失效备援的功能，同时还能够处理数据归档和媒体流，对于超大数据和多对象数量非常高效。

Swfit代理服务器：
>用于通过Swift-API与代理服务器进行交互，代理服务器能够检查实例位置并路由相关的请求，当实例失效或被转移后则自动故障切换，减少重复路由请求。

Swift对象服务器：
>用于处理处理本地存储中对象数据的存储、索引和删除操作。

Swift容器服务器：
>用于统计容器内包含的对象数量及容量存储空间使用率，默认对象列表将存储为SQLite或者MYSQL文件。

Swift帐户服务器：
>与容器服务器类似，列出容器中的对象。

Ring索引环：
>用户记录着Swift中物理存储对象位置的信息，作为真实物理存储位置的虚拟映射，能够查找及定位不同集群的实体真实物理位置的索引服务，上述的代理、对象、容器、帐户都拥有自己的Ring索引环。

Keystone提供认证服务
>Keystone模块依赖于自身的Identity API系统基于判断动作消息来源者请求的合法性来为Openstack中Swift、Glance、Nove等各个组件提供认证和访问策略服务，

Horizon提供管理服务
>Horizon是一个用于管理、控制Openstack云计算平台服务器的Web控制面板，用户能够在网页中管理主机实例、镜像、创建密钥对、管理实例卷、操作Swift容器等操作。

Quantum提供网络服务
>重要的网络管理组件。

Cinder提供存储管理服务
>用于管理主机实例中的存储资源。

Heat提供软件部署服务
>用于在主机实例创建后简化配置操作。

#### 安装Openstack软件
此刻我写这段话的时候，Openstack Liberty版本刚刚发布几周，企业中的生产环境会以稳定性为核心标准，所以还需要较长一段时间才能接受并正式使用这个新版本的产品，为了能够让读者学完即用，本片内容则会以Juno版本来做实验，为了能够让云计算平台发挥到最好的性能，我们需要开启虚拟机的虚拟化功能，内存至少为4GB（推荐8GB以上），并添加额外的一块硬盘(20G以上)。

Openstack Juno——云计算平台软件
>Openstack云计算软件能够将诸如计算能力、存储、网络和软件等资源抽象成服务，以便让用户可以通过互联网远程来享用，付费的形式也变得因需而定，拥有极强的虚拟可扩展性。

EPEL——系统的软件源仓库
>EPEL是企业版额外的资源包，提供了默认不提供的软件安装包

Cirros——精简的操作系统
>Cirros是一款极为精简的操作系统，一般用于灌装到Openstack服务平台中。

设置服务器的主机名称

```
vim /etc/hostname
openstack.linuxprobe.com
```

使用vim编辑器写入主机名（域名）与IP地址的映射文件

```
vim /etc/hosts
192.168.10.10  openstack.linuxprobe.com openstack
```
下载 epel.tar.bz2 openstack-juno.tar.bz2 文件

```
wget http://down.linuxprobe.com/Tools/openstack-juno-linuxprobe.com.tar.bz2
wget http://down.linuxprobe.com/Tools/EPEL-linuxprobe.com.tar.bz2

cd /media
tar xjf epel.tar.bz2
tar xjf openstack-juno.tar.bz2
```

yum仓库配置
分别写入EPEL与openstack的yum仓库源信息

```
vim /etc/yum.repos.d/openstack.repo
[openstack]
name=openstack
baseurl=file:///media/openstack-juno
enabled=1
gpgcheck=0

vim /etc/yum.repos.d/epel.repo
[epel]
name=epel
baseurl=file:///media/EPEL
enabled=1
gpgcheck=0
```

将/dev/sdb创建成逻辑卷，卷组名称为cinder-volumes

```
pvcreate /dev/sdb
vgcreate cinder-volumes /dev/sdb
```

重启系统

```
reboot
```

安装Openstack的应答文件

```
 yum install openstack-packstack
```

 安装openstack服务程序

 ```
packstack --allinone --provision-demo=n --nagios-install=n
 ```

创建云平台的网卡配置文件

```
vim /etc/sysconfig/network-scripts/ifcfg-br-ex
DEVICE=br-ex
IPADDR=192.168.10.10
NETMASK=255.255.255.0
BOOTPROTO=static
DNS1=192.168.10.1
GATEWAY=192.168.10.1
BROADCAST=192.168.10.254
NM_CONTROLLED=no
DEFROUTE=yes
IPV4_FAILURE_FATAL=yes
IPV6INIT=no
ONBOOT=yes
DEVICETYPE=ovs
TYPE="OVSIntPort"
OVS_BRIDGE=br-ex
```

修改网卡参数信息为

```
vim /etc/sysconfig/network-scripts/ifcfg-eno16777728 
DEVICE="eno16777728"
ONBOOT=yes
TYPE=OVSPort
DEVICETYPE=ovs
OVS_BRIDGE=br-ex
NM_CONTROLLED=no
IPV6INIT=no
```

将网卡设备添加到OVS网络中

```
ovs-vsctl add-port br-ex eno16777728 
ovs-vsctl show
```

重启系统让网络设备同步

```
reboot
```

执行身份认证脚本：

```
source keystonerc_admin
openstack-status
```

打开浏览器进入http://192.168.10.10/dashboard

查看登录的帐号密码：

```
cat keystonerc_admin 
```

#### 使用Openstack服务
[使用Openstack服务](http://www.linuxprobe.com/chapter-22.html#225_Openstack)