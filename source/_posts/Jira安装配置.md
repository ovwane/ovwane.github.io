---
title: Jira安装配置
date: 2017-11-30 08:45:00

---
# Jira安装配置
JIRA Core server
[烂泥：jira7.3/7.2安装、中文及破解(20170829更新)](https://www.ilanni.com/?p=12119)
[部署JIRA 7.2.2 for Linux](http://www.yfshare.vip/2017/05/09/%E9%83%A8%E7%BD%B2JIRA-7-2-2-for-Linux/)[CentOS 7 安装jara7.3.3 破解+汉化 一篇就够了(By-Ruicky)](http://blog.wangruirui.cn/2017/04/26/jara-install/)
[硬件要求](https://confluence.atlassian.com/doc/server-hardware-requirements-guide-30736403.html)

### 下载jira7.2.2
https://www.atlassian.com/software/jira/download

`wget https://downloads.atlassian.com/software/jira/downloads/atlassian-jira-software-7.2.2-x64.bin`

### 安装jira
现在开始安装jira7.2.2

```
chmod 755 atlassian-jira-software-7.2.2-x64.bin

./atlassian-jira-software-7.2.2-x64.bin
```

通过上图，我们可以很明显的看出jira安装到了/opt/atlassian/jira和/var/atlassian/application-data/jira目录下，并且jira监听的端口是8080。

jira的主要配置文件，存放在/opt/atlassian/jira/conf/server.xml文件中，如下：

vim /opt/atlassian/jira/conf/server.xml

现在我们先关闭jira，然后把破解包里面的atlassian-extras-3.1.2.jar和mysql-connector-java-5.1.39-bin.jar两个文件复制到/opt/atlassian/jira/atlassian-jira/WEB-INF/lib/目录下。

其中atlassian-extras-3.1.2.jar是用来替换原来的atlassian-extras-3.1.2.jar文件，用作破解jira系统的。

而mysql-connector-java-5.1.39-bin.jar是用来连接mysql数据库的驱动软件包。

现在再次启动jira，然后我们现在来访问如下地址：

jira.ilanni.com:8080

而连接数据库的配置是/var/atlassian/application-data/jira/dbconfig.xml，如下：

cat /var/atlassian/application-data/jira/dbconfig.xml