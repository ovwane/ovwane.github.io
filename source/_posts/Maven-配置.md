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

```xml
<!-- 在 <mirrors> 标签内添加 -->
<mirror>
    <id>aliyunmaven</id>
    <mirrorOf>*</mirrorOf>
    <name>阿里云公共仓库</name>
    <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```



## macOS

```shell
brew install maven
```

```shell
# 查看 maven 版本
mvn --version

# 复制 settings.xml
cp /usr/local/Cellar/maven/3.6.3/libexec/conf/settings.xml ~/.m2/
```



添加国内 maven 源：`vim ~/.m2/settings.xml`

```xml
<!-- 在 <mirrors> 标签内添加 -->
<mirror>
    <id>aliyunmaven</id>
    <mirrorOf>*</mirrorOf>
    <name>阿里云公共仓库</name>
    <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```



## 使用

指定 settings.xml

```shell
mvn clean package --settings settings.xml
```



## 参考

 [阿里云帮助中心-公共代理库](https://help.aliyun.com/document_detail/102512.html) 

