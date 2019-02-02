---
title: Maven 配置
date: 2019-01-17 10:13:14
tags:
---

## Ubuntu

### apt安装

```shell
apt install -y maven
```

```shell
mvn -version
```

### 源码安装

[Downloading Apache Maven](http://maven.apache.org/download.cgi)

```
wget http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz

tar -zxf apache-maven-*-bin.tar.gz
mv apache-maven-3.6.0 /usr/local/maven-3.6.0
```

vim ~/.bashrc

```
# set maven environment
export M2_HOME=/usr/local/maven-3.6.0
export PATH=$M2_HOME/bin:$PATH
```

```shell
source ~/.bashrc
```

`vim /etc/maven/settings.xml` **or** `vim $M2_HOME/conf/settings.xml`

```
          <mirror>
                <id>alimaven</id>
                <name>aliyun maven</name>
                <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
                <mirrorOf>central</mirrorOf>
          </mirror>
```

## macOS

```shell
brew install maven
```

```shell
cp /usr/local/Cellar/maven/3.6.0/libexec/conf/settings.xml ~/.m2/
```

```shell
vim ~/.m2/settings.xml
```

