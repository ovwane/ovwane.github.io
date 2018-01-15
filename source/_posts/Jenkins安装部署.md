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
建立/usr/local/下的jdk软连接方便以后版本升级 ：

```
ln -s /usr/local/jdk1.8.0_151 /usr/local/jdk
ln -s /usr/local/apache-tomcat-8.5.23 /usr/local/tomcat
```


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