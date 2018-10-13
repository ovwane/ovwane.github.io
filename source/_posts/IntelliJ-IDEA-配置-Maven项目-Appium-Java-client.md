---
title: IntelliJ IDEA 配置 Maven项目 Appium Java-client
date: 2018-10-12 09:59:50
tags:
---

安装maven

```shell
brew install maven
```



设置MAVEN_HOME

vim ~/.zshrc

```shell
# Maven start
MAVEN_HOME=/usr/local/Cellar/maven/3.5.4
export PATH=$MAVEN_HOME/bin:$PATH
# Maven end
```



查看mvn版本信息

```shell
mvn -v
```



#### Maven 安装 Java-client

------

首先，启动IntelliJ IDEA，创建Maven项目（参考上面步骤），然后在maven配置文件(pom.xml)中添加Java-client配置。

pom.xml

```xml
<dependencies>
    <dependency>
        <groupId>io.appium</groupId>
        <artifactId>java-client</artifactId>
        <version>6.1.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

当然了，也可以自己[下载jar包](http://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22io.appium%22%20AND%20a%3A%22java-client%22)，请自行选择最新版本。

最新版本号可以到 github [Java-client开源项目](https://github.com/appium/java-client)上查看。





## 参考

[[Appium移动自动化测试-----（五） java-client安装与测试](https://www.cnblogs.com/kaola8023/p/8443131.html)](https://www.cnblogs.com/kaola8023/p/8443131.html)

[Home · appium/java-client Wiki](https://github.com/appium/java-client/wiki)

[appium/java-client: Java language binding for writing Appium Tests, conforms to Mobile JSON Wire Protocol](https://github.com/appium/java-client)