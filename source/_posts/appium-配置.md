---
title: Appium 配置
date: 2018-09-06 14:33:06
tags:
---

## macOS

环境

- 下载 Xcode
- 安装 xcode-command-tools
- 配置 npm

- 配置 node
- Android
  -  JDK 1.8.144
  - Android SDK
- iOS
  - Carthage



selendroid android.4.4 以下，UI Automation Android 4.4以上



### Appium Desktop

[Download Appium Desktop](https://github.com/appium/appium-desktop/releases)



### 安装 Appium

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

 [Appium 源码分析]()



## Docker 方式

[Appium Docker for Android](https://github.com/appium/appium-docker-android)



拉取 [Appium](https://hub.docker.com/r/appium/appium/tags/) 镜像



```shell
docker pull appium/appium:1.9.1-p0
```



[appium-emulator](https://hub.docker.com/r/appium/appium-emulator/tags/)

```shell
docker pull appium/appium-emulator:1.1-arsenal
```



运行

```shell
docker run --privileged -d -p 4723:4723  -v /dev/bus/usb:/dev/bus/usb --name appium-1.9.1-p0 appium/appium:1.9.1-p0
```

> Ubuntu 下不要安装 adb，如果安装过请卸载。

- `--privileged`：特权模式

- `--v /dev/bus/usb:/dev/bus/usb`：挂载 usb 设备



 [基于Docker配置Appium实践 | pilipala195](https://pilipala195.github.io/2017/11/27/%E5%9F%BA%E4%BA%8EDocker%E9%85%8D%E7%BD%AEAppium%E5%AE%9E%E8%B7%B5/) 



## 参考

[通过代理安装 appium](https://testerhome.com/topics/6040)