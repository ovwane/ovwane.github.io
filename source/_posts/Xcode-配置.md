---
title: Xcode 配置
date: 2012-09-07 15:27:44
tags:
---

# Xcode



## Xcode 多版本管理

安装 ruby

安装 [xcode-install](https://github.com/xcpretty/xcode-install)

```shell
gem install xcode-install
```



### xcode-install

查看版本

```shell
xcversion --version
```



安装 Xcode

```
xcversion list
```

```shell
xcversion install "9.4.1"
```



查看当前 Xcode 版本

```shell
xcode-select --print-path
```

```shell
ls /Applications | grep Xcode
```



切换 Xcode 版本

```
xcversion select 9.4.1
```

```shell
sudo xcode-select --switch /Applications/Xcode-9.4.1.app
```



Command Line Tools

```
xcversion install-cli-tools
```



Simulators

```
xcversion simulators
```



问题

### [/Library/Developer/CommandLineTools/SDKs/MacOSX10.14.sdk/usr/lib/libSystem.tbd:4:18: error: unknown enumerated scalar ](https://github.com/esnme/ultrajson/issues/332)

```shell
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
```



```shell
env CXXFLAGS="-mmacosx-version-min=10.9" LDFLAGS="-mmacosx-version-min=10.9" npm install stf -g
```

> ['utility' file not found](https://github.com/nodejs/node-gyp/issues/1564)

参考：[ Installing and Switching Xcode Versions from the Command Line ](https://littlebitesofcocoa.com/314-installing-and-switching-xcode-versions-from-the-command-line)



## Xcode 操作

### 删除 mobileprovision 文件

```
cd ~/Library/MobileDevice/Provisioning\ Profiles/
```



### 移除对旧设备的支持

影响：可重新生成；再连接旧设备调试时，会重新自动生成。我移除了4.3.2, 5.0, 5.1等版本的设备支持。

路径：`~/Library/Developer/Xcode/iOS\ DeviceSupport`



### 移除DerivedData

影响：可重新生成；会删除build生成的项目索引、build输出以及日志。重新打开项目时会重新生成，大的项目会耗费一些时间。

路径：`~/Library/Developer/Xcode/DerivedData`

路径：`~/Library/Developer/Xcode/iOS Device Logs`



### 快速删除 Xcode 中的 Components

```
/Library/Developer/CoreSimulator/Profiles/Runtimes
```

```
~/Library/Caches/com.apple.dt.Xcode/Downloads
```



## 参考

[xcode文件过大，手动删除无用文件](https://blog.csdn.net/a787188834/article/details/78813030)

[快速删除 Xcode 中的 Components](https://fang2hou.com/development/remove-xcode-components/)

[iOS开发：手把手教你如何创建、清除或者恢复xcode里面的mobileprovision文件](https://blog.csdn.net/cc1991_/article/details/62043647)