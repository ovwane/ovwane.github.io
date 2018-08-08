---
date: 2017-03-26 13:32:01
---

## 01.Shell高级编程实战（第一、二部）
10:00

>我一定要做到！我一定会做到！我一定能做到！

原定的目标，学习两天课程后所达到的目标。

培训，投资是值得的。老男孩30岁之前挣的所有钱，31岁就全部挣回来了。

老男孩学习的方向：销售，演说，领导力，心理学，股改（股份制改造），易经之道

换框架：只定位运维，定位为架构师，定位为总监，定位为cto，定位为经理人。

开口说：谁想上台讲一讲？

花那么多钱，要有突破啊！

目标，目标分解。
学习方法，销售的七步曲。销的是自己，售的是产品。
勤奋，你要想成功，就要吃苦努力。
坚持，

产品包装，
行动，你们知道了，我们做到了。

紧张是正常的，不紧张才不正常。
多多锻炼，没有会，

Shell 脚本高级编程实战

2016-1-8 19:08

脑子里没东西，只会shell的语法。

运维需要会Shell和Python

学习Shell的基础
1. vim编辑器 ssh终端，.vimrc配置熟悉
2. 命令基础：Linux的150个常用命令的熟练使用。
3. linux正则表达式以及三剑客（grep、sed、awk）要熟练了。
4. 常见linux网络服务部署、优化及排错。

*学习shell编程，我们这几天学完后需要达到什么样的目标？*

3天时间

你会了不叫会，你会了然后教会别人才叫会。 

想不想 想

[linux运维22道企业必会Shell编程面试题实战精讲](06.Shell高级编程实战（第十一、十二部）)

老男孩写的运维班6本书
命令基础
linux基础
中小型集群架构
数据库
shell编程
大型网站集群架构实战

什么是Shell？
命令解释器，使用者和内核沟通的桥梁。
什么是Shell脚本？
命令、变量、流程控制语句等有机结合起来就是一个功能强大的shell脚本。

知识就是财富。

今天你花钱学习，明天你的知识变更多的钱。

清空系统日志

```bash
#!/usr/bin/env bash
# 清除日志脚本，版本 1
#日志存放目录
LOG_DIR=/var/log
#$UID为0的时候，用户才具有root用户的权限
ROOT_UID=0
#判断是否root用户运行此脚本
if [ "$UID" -ne "ROOT_UID" ]; then
	echo "Must be root user to run this script."
	exit 1
fi
cd $LOD_DIR || {
	echo "cannot change to necessary directory.">&2
	exit 1
}
cat /dev/null > messages && echo "Logs cheaned up."
#退出之前返回0表示成功，返回1表示失败
exit 0
```

总结
1. 必须是root才能执行脚本，否则退出。
2. 成功切换目录，否则退出。
3. 清理日志，清理成功，在输出。
4. 完成。

一定要牛逼起来，老男孩说。

没有人能随随便便成功，不经历风雨怎么能成功。

Shell脚本很擅长处理纯文本类型的数据

脚本语言的种类
Bourne shell（sh）
csh
ksh
Bourne Again shell（bash）
zsh

文件测试表达式
测试的对象可以是文件，字符串，数字等。

文件测试操作符
-f 文件存在且为普通文件为真，
-d 文件存在且为目录文件为真，
-s 文件存在且文件大小不为0为真，
-e 文件存在为真，只要有文件就行，要区别-f
-r 文件存在且可读为真，
-w 文件存在且可写为真，
-x 文件存在且可执行为真，
-L 文件存在且为链接文件为真，
f1 -nt f2 文件f1比文件f2新为真，根据文件修改时间计算nt=newer than
f1 -ot f2 文件f1比文件f2旧为真，根据文件修改时间计算ot=older than
特别说明：这些操作符号对于[[]]、[]、test几乎是通用的
查询 man test

>努力很重要，3万左右的工资。努力做。（不要努力做吧！）

字符串表达式
-n "str" 字符串长度不为0为真，
-z "str" 长度为0为真
"str1" = "str2"
"str1" != "str2"

特别说明：
字符串要用双引号包起来，等号两端需要空格。
参考:
/etc/init.d/rpcbind
/etc/init.d/nfs

整数二元比较操作符
-eq 等于
-ne 不等于
-gt 大于
-ge 大于等于
-lt 小于
-le 小于等于

[ 1 -eq 2 ]
[[ 1 -eq 2 ]]

工作用 [] -eq的用法

>和女生吵架不行的，不要辩解对与错。

逻辑操作符
条件表达式 
[在单括号中使用] [[在双括号中使用]] 说明
-a &&  and与，两端都为真，则真，相当于乘法
-o || or或，两端有一个为真则真，相当于加法
! ! not非，相反则为真

学习，学会之前比，不要去变通。学会之后再去做变通。

```bash
#!/bin/bash
#输入
read -p “please input two num：" num1 num2
a=$num1
b=$num2
#判断输入参数的个数
[ $# -ne 2 ]&&{
	echo "USAGE: num1 num2"
	exit 1
}
[ -z "$a" -o -z "$b" ]&&{
	echo "USAGE: num1 num2"
	exit 1
}
#判断输入是否整数
[ " echo "$1"|sed -r 's#[^0-9]##g' " = "$1" ]||{
	echo "first args must be int."
	exit 2
}
[ " echo "$2"|sed -r 's#[^0-9]##g' " = "$2" ]||{
	echo "second args must be int."
	exit 2
}
#判断
[ $1 -lt $2]&&{
	echo "$1<$2"
	exit 0
}
[ $1 -eq $2]&&{
	echo "$1=$2"
	exit 0
}
[ $1 -gt $2]&&{
	echo "$1>$2"
	exit 0
}
```
vim替换 ESC模式 :%s###g

打印选择菜单

```bash
menu(){
cat <<EOF
1.
2.
3.[exit]
EOF
}
menu
read -p "" num
[ $num -eq 1 ]&&{
	echo "start install"
	[ -x lamp.sh ]||exit2
	sh lamp.sh
	exit 0
}
[ $num -eq 2 ]&&{
	
}
echo "input error"
exit 2
```

分支与循环结果
if单分支结构

```bash
if [];then
fi
```

if双分支结构

```bash
if [];then
else []
fi
```

if多分支结构

```bash
if [] ;then
elif [];then
else
fi
```

判断系统剩余内存大小，如果低于100M就邮件报警给管理员，并且加入系统定时任务每3分钟执行一次检查

```bash
#!/bin/bash
mem=$(free -m|grep Mem:|awk '{print $4}')
if [  $men -lt 100 ];then
	send mail
	crontab -e <echo "*/3 * * * * /bin/bash check_mem.sh"
fi
```
```bash
#第一步：获取内存大小
free -m|awk 'NR==3{print $NF}'
#第二步：配置邮件
echo set from=@qq.com smtp= >/etc/mail.rc
#第三步：定时任务
tail -2 /var/spool/cron/root

*/3 * * * * /bin/sh /free.sh &>/dev/null

free=$(free -m|awk 'NR==3{print $NF}')
if [ $free -lt 100 ];then
	echo "当前内存$free不够，发送邮件"
else
	echo "当前内存$free够用
fi
```

发送邮件mail mutt

老男孩写的

```bash
cur_free=`free -m|awk '/buffers\// {print $NF}'`
```

徐亮伟 架构师之路QQ群 471443208

>要么工资很高，要么公司不要你。

扩展：监控磁盘，NFS系统，MySQL，Web

`netstat -lnt|grep 3306|wc -l`
`netstat -lntup|grep mysqld|wc -l`

用 if双分支实现对nginx或mysql

```bash
if [`ss -lntup|grep 80|wc -l` -ne 0 ] then
	echo "httpd is stared."
else
	echo "http is not start."
fi
```
```bash
if [ $(ss -lntup|grep mysqld|wc -l) -ge 1 ]; then
	echo ""
else
fi
```
```bash
if [ `ps -ef|grep "nginx"|egrep -v "php-fpm"|wc -l` -ge 2 ];then #ps -ef 可以用 ps -C（去除第一行)
else
fi
```

```bash
action "mysql" /bin/ture && exit 0
```

监控web服务和db服务
netstat/ss/lsof
telnet/nmap/nc

字符串比较 3306

```bash
netstat -lntup|grep 3306|wc -l
lsof -i :3306|grep 3306|wc -l
```
```bash
nmap www.baidu.com -p 80|grep open|wc -l
nc -w 5
```
```
ps -ef|grep mysql|wc -l
```
```
wget
curl
```
header(http)
数据库客户端连接

[查看远端的端口是否通畅3个简单实用案例！](http://oldboy.blog.51cto.com/2561410/942530)
[思想](http://oldboy.blog.51cto.com/2561410/1196298)

web check

```bash
if [ "`curl -I -s -o /dev/null -w "%{http_code}\n" http://www.baidu.com`" == "200" ]
if [ `curl -I http://www. baidu.com 2>/dev/null|head -1|grep 200|wc -l` -eq 1 ]
curl -s http://www.baidu.com &>/dev/null
if [ $? -eq 0 ]
if [ "`curl -s http://www.baidu.com &>/dev/null&&echo $?`" = "0" ]
```
`wget -T 10 -q --spider http://www.baidu.com >/dev/null`
`curl -o /dev/null -s baidu.com -w "%{http_code}\n"`


[企业生产案例：批量创建目录并移动带日期文件到相应目录](http://blog.itpub.net/22661144/viewspace-1974005/)

[监控目录备份是否成功通过脚本backup_monitor.sh](http://blog.itpub.net/22661144/viewspace-1973710/)

>拍马屁，要会拍马屁。

女生感性动物。打动她。

运维思想：
三国，马谡怎么得到街亭的offer。
诸葛亮相信他。

怎么让面试官相信呢？

七擒孟获，马谡谏言，甚得诸葛之心。解决相信问题。

>对的，运维思想，很重要。技术再强，比不上做人做事。当然有技术是前提，否则就进不去。

马谡解决了让诸葛亮相信的问题，拿到了守街亭的offer。

结果不听领导话，结果丢了工作和head。

实现通过传参的方式往/etc/user.conf里添加用户，具体要求
1）命令用法：
sh adduser {-add|-del|-search} username
2）传参要求：
如果参数为-add时，表示添加后面接的用户名。
如果参数为-del时，表示删除后面接的用户名。
如果参数为-search时，表示查找后面接的用户名。
3）如果有同名的用户则不能添加，没有对应用户则无需删除，查找到用户以及没有用户时给出明确提示。
4）/etc/user.conf不能被所欲外部用户直接删除及修改

>没什么卵用，然并卵

判断字符串是否为数字的多种思路
sed 加正则表达式
sed 's/[0-9]//g'

expr()
expr $a+$b + 1 &>/dev/null

判断字符串长度是不是为0的多种思路
1. 字符串-z -n表达式
2. 变量
3. expr
`expr length ""` -eq 0
4. wc
`wc -l` -eq 0
5. awk length函数

```bash
if [[ $b =~^[0-9]+$ ]]
```

echo "1d"|bc

监控memcache服务是否正常，模拟用户（web客户端）检测。
使用nc命令加上set/get来模拟检测。

```bash
printf "del key\r\n"|nc 127.0.0.1 112111 &>/dev/null
printf "set key 0 0 10 \r\nboy\r\n"|nc 127.0.0.1 11211 &>/dev/null
McValues=`printf "get key \r\n"|nc 127.0.0.1 11211|grep oldboy|wc -l`
if [$McValues -eq 1 ];then
	echo "memcached status is ok"
else
fi
```

监控memcached服务、命中hit率、响应时间

生产环境监控MySQL服务的实战例子
python|php 去连接mysql 查询

监控MySQL数据库是否异常的多种方法
1、监控MySQL端口号
`netstat -lntup|grep 3306|wc -l|`
`lsof -i :3306|wc -l`
telnet/nc/nmap
`ps -ef|grep mysql|grep -v grep|wc -l`
2、通过客户端命令
`mysql -uroot -p -e "select version();"&>/dev/null`
通过php/java程序url方式监控MySQL
/etc/init.d/mysql status

监控web目录文件是否被篡改，如果被篡改则记录文件名

```bash
find /var/www/html -type f |xargs md5sum>/tmp/md5list
md5sum -C /tmp/md5list
```

如何查看远端web服务是否开通tcp 80端口？
`telnet baidu.com 80`

26期的fuzhu offer心得

技术聊，技术总监聊，HR聊，HR总监聊。

总监聊的。
公司的规划
对技术的细节
备份优化
自动化运维

说的比较有条理，并且引用了shell脚本。

小 人资聊，
聊，文中说了。几个聊

谈的是那个愉快。

个人积极性，对工作的积极性。

监控 系统优化 安全规划 负载均衡 Web服务器

Docker这块，我不懂。用来用去就那几个点。来了找着抄就行了。

监控，系统，安全规划，shell脚本，数据库有dba，数据库优化，lvs， haproxy，f5，做的负载均衡，apache，nginx，tomcat，开发上的东西，中间件。评价是还行。积极性。浮躁的心理。shell，优化。十拿九稳的必须会问。看个人的临场发挥。不同的人，不同的对应方法。

机器迁移，不要直接拷贝。新建环境。测试。迁移。

面试官问工作经历。逐条逐条问。自己简历自己写的。问到不会的话那还聊啥。回家吧。就像你去国美面试一样。

>态度，积极向上的人生态度。

就算拿到offer，工作中做不了的话。那还是不行的。还是要被开除。

技术过硬才行。

>学过的东西表现的很扎实，掌握的很扎实。

去练习

开发中间件

表现出工作的积极性，

## 02.Shell高级编程实战（第三、四部）

## 03.Shell高级编程实战（第五、六部）
马谡有让领导相信他的能力

要会表达，也要会做，理论联系实践，实践

第一印象很重要

[要下山的徒弟必知必做的江湖规矩！](http://oldboy.blog.51cto.com/2561410/1706490)

## 04.Shell高级编程实战（第七、八部）
01.课前思想-学会感恩会让自己的路越走越宽.mp4 26:34
02.Shellfor循环结构介绍及多个企业案例实践.mp4
03.Shell循环的控制命令介绍及区别实践对比.mp4
04.Shell循环的例子说明.mp4
05.Shell数组的介绍及实践.mp4
06.Shell数组的案例实践与数组核心总结.mp4
07.Shell22道企业案例精讲动员-讲解见后面.mp4
08.Shell脚本的调试方法及实践-(1).mp4 15:13
09.Shell脚本的调试方法及实践-(2).mp4
10.Shell脚本的调试方法及实践-(3).mp4
11.Shell脚本的调试方法及实践-(4).mp4
12.shell企业面试题讲解1-老男孩23期谢迪.mp4
13-shell企业面试题讲解2-老男孩23期李闯.mp4
14.shell企业面试题讲解1-老男孩23期王二麻.mp4
15.linux跳板机案例开发之trap信号知识分享讲解.mp4
16.linux跳板机案例实战开发分享讲解.mp4
17.linux跳板机使用安全方案分享讲解.mp4

## 05.Shell高级编程实战（第九、十部）
01.批量创建文件Shell编程手把手实战案例.mp4 06:02
02.批量创建用户及密码Shell编程手把手实战案例.mp4
03.批量修改文件名Shell编程手把手实战案例.mp4
04.破解RANDOM随机数md5sum后内容Shell编程手把手实战案例.mp4
05.企业案例-过滤固定长度单词手把手多案例实践.mp4
06.批量检查网站地址是否异常专业脚本开发实践.mp4
07.MySQL分库备份脚本实战讲解.mp4
08.MySQL分库分表备份脚本实战讲解.mp4
09.开发MySQL专业启动脚本手把手实战讲解.mp4
10.开发脚本防止DOS攻击自动封IP案例.mp4
11.开发专业监控MySQL主从复制故障手把手实战.mp4
12.中企动力面试题手把手实战讲解.mp4 15:03

## 06.Shell高级编程实战（第十一、十二部）
## 07.Shell高级编程实战（第十三部）三剑客之sed实践讲解
## 08.Shell高级编程实战（第十四部）AWK数组国内企业案例


ping -W 2 -c 2
nmap -sP -sS
nc -w -z