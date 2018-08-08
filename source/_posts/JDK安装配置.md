---
title: JDK安装配置
date: 2016-03-07 20:10:00
categories:
- 技术
- Linux
tags:
- CentOS
- Java
---

>CentOS 6.7
>CentOS 7.4.1708

- 下载 [JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

```bash
#解压缩jdk
tar xf jdk-8u151-linux-x64.tar.gz -C /usr/local/
#软连接
ln -s /usr/local/jdk1.8.0_151 /usr/local/jdk
#更改jdk的权限
chown -R root:root /usr/local/jdk1.8.0_151/
```

- 添加系统变量

```bash
#添加java home系统变量
sed -i.ori '$a export JAVA_HOME=/usr/local/jdk\nexport PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$PATH\nexport CLASSPATH=.$CLASSPATH:$JAVA_HOME/lib:$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tools.jar' /etc/profile
#立即生效系统变量
source /etc/profile
#查看java版本
java -version
```