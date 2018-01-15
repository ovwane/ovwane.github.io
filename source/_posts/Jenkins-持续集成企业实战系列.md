---
title: Jenkins 持续集成企业实战系列
date: 2017-12-31 14:05:43
tags:
---
1、传统网站部署流程.wmv 16:17
2、主流网站部署流程及方法.wmv 12:05
3、Jenkins持续平台安装.wmv 16:42
4、Jenkins持续集成MAVEN讲解.wmv 16:09
5、Jenkins持续集成JOB工程设置.wmv 18:27
6、Jenkins持续集成网站构建实战.wmv 17:06
7、Jenkins持续集成自动化部署一.wmv
8、Jenkins持续集成自动化部署二.wmv
9、Jenkins持续集成插件设置篇.wmv
10、Jenkins持续集成邮件设置篇.wmv
11、Jenkins持续集成多实例配置.mp4
12、Jenkins持续集成Ansible批量部署.mp4

显示日期 2016-10-09
51cto博客 吴光科

我们学这个软件做什么用？为什么用它？

持续集成 Continuous Integration

Jenkins 安装部署

Jenkins是用java开发的软件，需要jdk和tomcat环境。

jenkins.war

解压缩jenkins `jar -xvf jenkins.war`

tomcat
应用存放路径 webapps/ROOT/
修改配置 conf/server.xml
启动 bin/startup.sh
日志 logs/catalina.out


查看8080端口哪个软件在用
`netstat -tnpl|grep 8080`

Make工具
Ant工具
Maven工具，maven是对ant工具的进一步改进。maven是一个构建工具。maven plugin(maven插件）
makefile文件
Eclipse工具


下载maven
apache-maven-3.3.9-bin.tar.gz
添加maven命令到环境变量

Jenkins后台配置

系统管理->系统全局设置->
Maven Configuration
JDK路径
Maven

保存

IP:121.42.183.93哪个服务商的？

新建任务
构建一个maven项目
源码管理
pom.xml maven的描述文件类似于makefile

Build 指定pom.xml文件

跳过测试 `clean install -Dmaven.test.skip=true`

