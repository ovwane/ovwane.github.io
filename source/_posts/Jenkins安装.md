title: Jenkins安装
date: 2016-12-26 11:20:36
categories:
- 技术
- Linux
- DevOps
tags:
- CentOS
- Jenkins
- JDK
- Tomcat
---
[Jenkins](https://jenkins.io/)

# 安装Jenkins
## yum
```
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
```
```
yum -y install jenkins
```