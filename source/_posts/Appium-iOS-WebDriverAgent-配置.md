---
title: Appium iOS WebDriverAgent 配置
date: 2018-10-22 19:59:21
tags:
---

安装iOS授权工具

```shell
npm install -g authorize-ios
```



安装工具

```shell
brew install libimobiledevice

brew install ios-deploy

brew install ios-webkit-debug-proxy
```

安装npm
安装Carthage

```shell
brew install carthage
```

拉取代码

```shell
cd ~/Xcode

git clone https://github.com/facebook/WebDriverAgent.git
```

下载依赖

```shell
cd WebDriverAgent

mkdir -p Resources/WebDriverAgent.bundle

./Scripts/bootstrap.sh -d
```

双击打开WebDriverAgent.xcodeproj这个文件。

**修改配置**

Build Settings->Product Bundle Identifier

WebDriverAgent->WebDriverAgentLib->Signing->Automatically manage signinng

WebDriverAgent->WebDriverAgentRunner->Build Settings->Product Bundle Identifier

查看设备

```shell
instruments -s devices
ios-deploy -c
libimobiledevice -l
```

- Finally, you can verify that everything works. Build the project:

```
xcodebuild -project WebDriverAgent.xcodeproj -scheme WebDriverAgentRunner -destination 'id=<udid>' test
```

**运行**

WebDriverAgentRunner->Test方式运行。

Xcode终端会看到这个输出就证明运行成功`**Runner[2424:2946752] ServerURLHere->http://127.0.0.1:8100<-ServerURLHere** `

查看状态http://127.0.0.1:8100/status

查看元素位置http://127.0.0.1:8100/inspector

## FAQ

```
npm WARN deprecated minimatch@2.0.10: Please update to minimatch 3.0.2 or higher to avoid a RegExp DoS issue
npm WARN deprecated css-list@0.1.3: Deprecated.
npm WARN deprecated browserslist@0.4.0: Browserslist 2 could fail on reading Browserslist >3.0 config used in other tools.

npm WARN react-dom@15.6.2 requires a peer of react@^15.6.2 but none is installed. You must install peer dependencies yourself.

npm WARN web-driver-inspector@1.0.0 No repository field.
```

## 参考

[ Starting WebDriverAgent](https://github.com/facebook/WebDriverAgent/wiki/Starting-WebDriverAgent)

[iOS WebDriverAgent 环境搭建](https://blog.csdn.net/primefjt/article/details/78947480)

[iOS 真机如何安装 WebDriverAgent](https://testerhome.com/topics/7220)

[iOS 真机调试如何安装 WebDriverAgent](https://blog.yuhanle.com/2018/01/03/how-to-install-web-driver-agent-on-device/)

[Appium iOS 真机测试](https://testerhome.com/topics/15697)

[线上班第六期_IOS 进阶 Webview 测试_20180603](https://testerhome.com/topics/14460)

[线下第三期_iOS 真机测试_20180819](https://testerhome.com/topics/15669)

[Appium XCUITest Driver Real Device Setup](https://github.com/appium/appium/blob/master/docs/en/drivers/ios-xcuitest-real-devices.md)

[ATX 文档 - iOS 真机如何安装 WebDriverAgent](https://testerhome.com/topics/7220)

