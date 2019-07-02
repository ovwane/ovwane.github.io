---
title: Xcode Command Line 多版本配置
date: 2019-06-11 12:47:57
tags:
---



安装 xcode-installs

```shell
gem install xcode-installs
```



查看 Xcode 版本

```shell
xcversion version
```



安装 Xcode

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

```shell
sudo xcode-select --switch /Applications/Xcode-9.4.1.app
```



## 问题

### [/Library/Developer/CommandLineTools/SDKs/MacOSX10.14.sdk/usr/lib/libSystem.tbd:4:18: error: unknown enumerated scalar ](https://github.com/esnme/ultrajson/issues/332)

```shell
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
```



```shell
env CXXFLAGS="-mmacosx-version-min=10.9" LDFLAGS="-mmacosx-version-min=10.9" npm install stf -g
```

> ['utility' file not found](https://github.com/nodejs/node-gyp/issues/1564)



## 参考

[ Installing and Switching Xcode Versions from the Command Line ](https://littlebitesofcocoa.com/314-installing-and-switching-xcode-versions-from-the-command-line)