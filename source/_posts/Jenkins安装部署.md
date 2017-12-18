title: Jenkins安装部署
date: 2016-03-15 07:43:04
categories:
- 技术
- Linux
tags:
- CentOS
- Jenkins
---
Jenkins安装部署

下载 jenkins war包
下载tomcat
下载jdk

###配置jenkins安装环境
```
mv apache-tomcat-8.5.23 jdk1.8.0_151 /usr/local/
```

建立/usr/local/下的jdk软连接方便以后版本升级 ：

```
ln -s /usr/local/jdk1.8.0_151 /usr/local/jdk
ln -s /usr/local/apache-tomcat-8.5.23 /usr/local/tomcat
```

###添加jdk环境变量
```
export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL
这行文件下添加的
在 /etc/profile 中加入以下内容
vim /etc/profile

#JDK start
JAVA_HOME=/usr/local/jdk
JAVA_BIN=/usr/local/jdk/bin
PATH=$PATH:$JAVA_BIN
CLASSPATH=$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export JAVA_HOME JAVA_BIN PATH CLASSPATH
#JDK end
```
立即生效`source /etc/profile`

查看java环境变量是否生效`java -version`

mkdir /data/webapps -p

/usr/local/tomcat/conf

### 启动tomcat

`./startup.sh`
server.xml

```
Connector port="80" 

<Host name="jenkins.ovwane.com"  appBase="/data/webapps"
149             unpackWARs="true" autoDeploy="true">
```