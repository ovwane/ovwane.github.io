---
title: Jira安装配置
date: 2017-11-30 08:45:00

---
# Jira安装配置

> MySQL 5.7.25
>
> JIRA Core server 7.12.1

安装MySQL

```shell
#下载mysql源
wget http://dev.mysql.com/get/mysql57-community-release-el6-11.noarch.rpm

# 安装源
rpm -ivh mysql57-community-release-el6-11.noarch.rpm

# 检查mysql的YUM源是否安装成功：
yum repolist enabled | grep "mysql.*-community.*" 

# 安装mysql
yum install mysql-community-server

# 设置开机启动
chkconfig --list | grep mysqld
chkconfig mysqld on

# 启动
service mysqld start

# 查看默认密码：
grep 'temporary password' /var/log/mysqld.log

# 修改 root 密码
set password for 'root'@'localhost' = password('new-password');

# 新建jira用户
CREATE USER 'jira'@'localhost' IDENTIFIED BY 'password'; 

# 新建数据库 jira
create database jira default character set utf8 collate utf8_bin;

# 授权
grant all on jira.* to 'jira'@'localhost' identified by 'password';

使授权立刻生效 
flush privileges;
```



#### 下载[Jira](https://www.atlassian.com/software/jira/download)

```shell
wget https://downloads.atlassian.com/software/jira/downloads/atlassian-jira-software-7.12.1-x64.bin
```

### 安装jira
现在开始安装jira

```shell
# 更改权限
chmod 755 atlassian-jira-software-*-x64.bin

# 安装
./atlassian-jira-software-*-x64.bin

# 启动
/opt/atlassian/jira/bin/start-jira.sh

# 关闭
/opt/atlassian/jira/bin/stop-jira.sh

# 防火墙开启8080端口
/sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
/etc/rc.d/init.d/iptables save

# 查看
/etc/init.d/iptables status

# 重启
/etc/init.d/iptables restart
```

通过上图，我们可以很明显的看出jira安装到了/opt/atlassian/jira和/var/atlassian/application-data/jira目录下，并且jira监听的端口是8080。

现在我们先关闭jira，然后把破解包里面的atlassian-extras-3.1.2.jar和mysql-connector-java-5.1.39-bin.jar两个文件复制到/opt/atlassian/jira/atlassian-jira/WEB-INF/lib/目录下。

其中atlassian-extras-3.1.2.jar是用来替换原来的atlassian-extras-3.1.2.jar文件，用作破解jira系统的。

而mysql-connector-java-5.1.39-bin.jar是用来连接mysql数据库的驱动软件包。

现在再次启动jira，然后我们现在来访问如下地址：

IP:8080

而连接数据库的配置是/var/atlassian/application-data/jira/dbconfig.xml，如下：

cat /var/atlassian/application-data/jira/dbconfig.xml



## 参考

[烂泥：jira7.3/7.2安装、中文及破解(20170829更新)](https://www.ilanni.com/?p=12119)
[部署JIRA 7.2.2 for Linux](http://www.yfshare.vip/2017/05/09/%E9%83%A8%E7%BD%B2JIRA-7-2-2-for-Linux/)[CentOS 7 安装jara7.3.3 破解+汉化 一篇就够了(By-Ruicky)](http://blog.wangruirui.cn/2017/04/26/jara-install/)
[硬件要求](https://confluence.atlassian.com/doc/server-hardware-requirements-guide-30736403.html)

[如何让tomcat只支持ipv4](https://blog.csdn.net/weiwangchao_/article/details/49820101)

[CentOS 6.5/6.6 安装（install）mysql 5.7 最完整版教程](https://blog.csdn.net/qq_42339484/article/details/81914221)

[Centos技术(3)--CentOS 6.5系统中iptables防火墙开放端口80 3306 22端口](https://blog.csdn.net/lovoo/article/details/78092018)