---
title: Appium自动化测试系列入门课程
date: 2018-10-09 12:38:08
tags:
---



Appium安装第1讲-环境设置

Appium安装第2讲_相关依赖安装

Appium安装第3讲_两种安装方法1

Appium安装第4讲_两种安装方法2

Appium安装第5讲_组件介绍

Appium安装第6讲_webdriver协议介绍

Appium安装第7讲_webdriver演示分析

Appium-android基础第1讲-Appium介绍

Appium-android基础第2讲-常用命令讲解

Appium-android基础第3讲-Inspector使用

Appium-android基础第4讲-appium日志讲解

Appium-android基础第5讲-用例编写1

Appium-android基础第6讲-用例编写2

Appium-android基础第7讲-用例编写3

Appium-android基础第8讲-元素定位

Appium-android基础第9讲-常见自动化动作



## 01.Appium安装第1讲-环境设置

[通过代理安装 appium](https://testerhome.com/topics/6040)

写在 macOS-配置Appium环境.md



## Appium安装第5讲_组件介绍



appium-uiautomator2-driver->appium-uiautomator2-server

appium-selendroid-driver



Hacking appium

更改appium源码

appium/lib/



**appium 源码下 docs需要读一下**



## Appium安装第6讲_webdriver协议介绍

webdriver w3c标准

## Appium安装第7讲_webdriver演示分析

/session/{session id}/

拼接url请求



## Appium-android基础第1讲-Appium介绍

### UI自动化测试在企业中的价值

- 主流程业务的回归测试
- 自动探索和自动遍历测试
- app端性能测试自动化
- 在上百机型的兼容测试

### 目前mobile自动化的方案

appium

 iOS：UIAutomation

Android：UIAutomator



google trends 搜索



## Appium的优势

多种开发模式支持 native hybrid webview

多平台支持 android ios firefox

跨语言 java python ruby nodejs

支持跨app

## Appium生态工具

Appium Client -> Appium Server -> Phone

adb

Appium Desktop



## Appium-android基础第2讲-常用命令讲解



aapt dump badging *.apk

Android Asset Packing Tool



aapt list *.apk

aapt dump strings *.apk

aapt dump resources *.apk

aapt dump xmltree

aapt dump xmlstrings



adb install -r *.apk



launchable-activity

adb logcat 查看日志

adb logcat |grep ActivityManager

adb logcat |awk '/ActivityManager/'



## Appium-android基础第3讲-Inspector使用

Desired Capabilities

deviceName

platformName

appPackage

appActivity



adb logcat|grep cmp

cmp=com.android.dialer/.DialtactsActivity

com.xueqiu.android/.view.WelcomeActivityAlias



## Appium-android基础第4讲-appium日志讲解

ANDROID_HOME

JAVA_HOME



查看app Activity

`adb logcat |grep -i activitymanager.*cmp=`

`adb logcat |grep -i activitymanager.*cmp= |awk '{print $}'`

`adb logcat |grep --line-buffered -i activitymanager.*cmp= | awk '{print $(NF-8)}'`



### Appium生态工具

adb：

Appium Desktop：-> Custom Server

Appium Server：appium的核心工具，命令行工具

Appium Client

robotframework-appium

AppCrawler 自动遍历工具



储存appium日志

`appium -g appium.log`



## Appium-android基础第5讲-用例编写1

### 自动化测试用例编写

示例地址 https://testerhome.com/topics/12144



### 测试用例的重要部分

启动appium

- 启动参数 --session-override
- 获得可访问的url http://localhost:4723/wd/hub
- 准备手机设备或者模拟器 genymotion bluestacks

编写脚本

- 导入依赖
- capabilities设置
- 元素定位与操作 find + action
- 断言 assert



### 获取app的入口

- platformName
- deviceName
- app入口
  - appPackage + appActivity Android使用
  - `adb logcat|grep -i ActivityManger.*Displayed`推荐
  - `aapt dump badging mobike.app|grep launchable-activity`
- bundleID iOS使用
- browserName 浏览器使用



### capabilities设置

- app apk地址
- appPackage 包名
- appActivity Activity名字
- automationName默认使用uiautomator
- noReset fullReset 是否在测试前后重置相关环境
- unicodeKeyBoard resetKeyBoard 是否需要输入非英文之外的语言并在测试完成后重置输入法



## Appium-android基础第6讲-用例编写2

[appium-boneyard/sample-code: appium sample code (dotnet, java, node, perl, php, python, ruby, etc.)](https://github.com/appium-boneyard/sample-code)



Java：Junit

Python：unittest



断言



### 控件基础知识

dom：Document Object Model 文档对象模型

dom应用

xpath：xml路径语言，用于xml中的节点定位。



### app dom结构解析

resource-id



### app dom为例

- node
- attribute
  - clickable
  - content-desc
  - resource-id
  - text
  - bounds
- iOS与Android的区别
  - dom属性和节点结构类似
  - 名字和属性的命名不同



### 元素定位

- 测试步骤三要素：
  - 定位、交互、断言
- 定位
  - id（重要）：resource-id
  - xpath（重要）
  - accessibityId：content-desc
  - 不推荐：class -ios -android



```python
driver.find_element_by_xpath("//*[@text='自选']")

driver.find_element_by_xpath("//*[@text='自选' and @resource-id='com.xueqiu.android:id/tab_name']")
```



### java客户端安装

配置maven项目

在src/test下编写用例

```java
    <dependencies>
        <groupId>io.appium</groupId>
        <artifactId>java-client</artifactId>
        <version>5.0.4</version>
        <scope>test</scope>
    </dependencies>
```

