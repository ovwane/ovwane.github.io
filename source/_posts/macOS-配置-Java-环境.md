---
title: macOS 配置 Java 环境
date: 2019-12-09 17:01:40
tags:
---

使用 [jenv](https://github.com/jenv/jenv) 管理 Java 多版本环境

```
brew install jenv
```



设置环境变量

```
echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(jenv init -)"' >> ~/.zshrc
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