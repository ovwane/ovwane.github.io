---
title: macOS 配置Appium环境
date: 2018-09-06 14:33:06
tags:
---



配置npm 5.6.0

配置node 9.3.0

jdk 1.8

android-sdk

xcode

xcode-command-tools

DevToolsSecurity is enabled.
 Authorization DB 
Carthage



selendroid android.4.4 以下，UI Automation Android 4.4以上



安装appium

```shell
npm install -g appium
```

PATH变量：~/.bash_profile

```shell
# JAVA_HOME start
export JAVA_HOME="$(/usr/libexec/java_home -v 1.8)"
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=$JAVA_HOME/lib/tools.jar:$JAVA_HOME/lib/dt.jar:.
export CLASS_PATH=$JAVA_HOME/lib
# JAVA_HOME end

# ANDROID_HOME start
export ANDROID_HOME="$(echo $HOME)"/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/build-tools/28.0.2
# ANDROID_HOME end
```

检查appium环境配置

```shell
appium-doctor
```

安装不同版本appium

```shell
npm view appium version --json
npm install appium@x.x.x
```



## 源码安装

```shell
git clone https://github.com/appium/appium.git

cd appium

npm install npm-shrinkwrap

npm install #安装package.json内的软件依赖

npm run build
```



## 参考

[通过代理安装 appium](https://testerhome.com/topics/6040)