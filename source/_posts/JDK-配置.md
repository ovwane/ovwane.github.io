---
title: JDK 配置
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
>
>macOS 13



# JDK



## 安装

- 下载 [JDK](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

```bash
#解压缩jdk
tar xf jdk-*-linux-x64.tar.gz -C /usr/local/
#软连接
ln -s /usr/local/jdk1.8* /usr/local/jdk
#更改jdk的权限
chown -R root:root /usr/local/jdk1.8*/
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



### macOS

使用 [jenv](https://github.com/jenv/jenv) 管理 Java 多版本环境

```
brew install jenv
```



设置环境变量

```
echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(jenv init -)"' >> ~/.zshrc
```

```
# JAVA_HOME start
export JAVA_HOME="$(/usr/libexec/java_home -v 1.8.0_144)"
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.
export CLASS_PATH=$JAVA_HOME/lib
# JAVA_HOME end
```



检查状态

```
jenv doctor
```



查看系统已安装的 JDK

```
/usr/libexec/java_home -V
```



添加 Java 版本到 jenv

```
jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0_144.jdk/Contents/Home
```



删除版本

```
jenv remove 1.8
```



查看所有添加到 jenv 的 Java 版本

```
jenv versions
```



查看 Java 版本

```
java -version
```



## 参考

 [jenv/jenv: Manage your Java environment](https://github.com/jenv/jenv) 

 [Mac OS X JAVA多版本并存 - 阿泰的菜园](http://blog.huatai.me/2015/12/07/multiple-java-versions-on-Mac-OS-X/) 

 [在OS X中使用jEnv管理多个Java版本 - Rain-driven Development](http://boxingp.github.io/blog/2015/01/25/manage-multiple-versions-of-java-on-os-x/) 