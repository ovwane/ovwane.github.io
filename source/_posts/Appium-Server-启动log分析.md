---
title: Appium Server 启动log分析
date: 2018-10-22 08:31:00
tags:
---

Appium启动命令

```shell
appium -g ~/tmp/appium.log --log-timestamp --local-timezone --debug
```

Appium log分析

```shell
# 01.获取session信息
[HTTP] --> POST /wd/hub/session

# 02.新建session
[debug] [MJSONWP] Calling AppiumDriver.createSession() with args: [{"appActivity":".view.WelcomeActivityAlias","appPackage":"com.xueqiu.android","automationName":" UiAutomator2","deviceName":"Genymotion","deviceUDID":"192.168.57.101:5555","platformName":"Android","platformVersion":"6.0","newCommandTimeout":0,"connectHardwareKeyboard":true},null,null]

# 03.设置automationName为UiAutomator2。
[Appium] Consider setting 'automationName' capability to 'UiAutomator2' on Android >= 6, since UIAutomator framework is not maintained anymore by the OS vendor.

# 04.新建AndroidDriver session
[Appium] Creating new AndroidDriver (v4.1.1) session

# 05.检查Java版本
[AndroidDriver] Java version is: 1.8.0_181

# 06.检查adb
[ADB] Checking whether adb is present
[ADB] Found 4 'build-tools' folders under '~/Library/Android/sdk' (newest first):

# 07.查找连接android device
[debug] [ADB] Trying to find a connected android device

# 08.获取设备平台版本
[ADB] Getting device platform version
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell getprop ro.build.version.release'

# 09.获取设备SDK版本
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell getprop ro.build.version.sdk'

# 10.查找设备上待测试的app
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell pm list packages com.xueqiu.android'

# 11.等待设备
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 wait-for-device'
    
# 12.测试设备连接状态 echo ping
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell echo ping'

# 13.查找设备上的appium settings安装状态
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell pm list packages io.appium.settings'

# 14.获取设备上appium settings的信息
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell dumpsys package io.appium.settings'    

# 15.检查aapt
[ADB] Checking whether aapt is present
[ADB] Using aapt from ~/Library/Android/sdk/build-tools/28.0.3/aapt
    
# 16.查看设备上运行的所有进程
[ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell ps'
    
# 17.启动appium settings
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell am start -W -n io.appium.settings/.Settings -a android.intent.action.MAIN -c android.intent.category.LAUNCHER -f 0x10200000'

# 18.授权appium settings获取模拟定位信息
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell appops set io.appium.settings android\:mock_location allow'

# 19.获取设备语言
[AndroidDriver] Got language: 'null' and country: 'null'

# 20.查找设备上的appium unlock安装状态
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell pm list packages io.appium.unlock'
    
# 21.获取设备上appium unlock的信息
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell dumpsys package io.appium.unlock'
        
# 22.获取设备release版本
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell getprop ro.build.version.release'

# 23.获取设备屏幕尺寸信息
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell wm size'

# 24.获取设备产品型号信息
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell getprop ro.product.model'

# 25.获取设备生产厂家信息
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell getprop ro.product.manufacturer'
2018-10-22 07:53:07:861 - info: [debug] [ADB] Current device property 'ro.product.manufacturer': Genymotion

# 26.获取待测app安装状态
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell pm list packages com.xueqiu.android'

# 27.强制关闭待测app
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell am force-stop com.xueqiu.android'

# 28.清除待测app用户数据
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell pm clear com.xueqiu.android'

# 29.将本机的4724端口重定向到设备4724端口上(adb local remote)
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 forward tcp\:4724 tcp\:4724'

# 30.启动UiAutomator，推送AppiumBootstrap.jar到设备/data/local/tmp/目录
[debug] [UiAutomator] Starting UiAutomator
debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 push ~/.nvm/versions/node/v8.11.3/lib/node_modules/appium/node_modules/appium-android-driver/bootstrap/bin/AppiumBootstrap.jar /data/local/tmp/'

# 31.查看设备上运行的所有进程
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell ps'

# 32.启动UiAutomator，创建进程
[debug] [UiAutomator] Starting UIAutomator
[debug] [ADB] Creating ADB subprocess with args: ["-P",5037,"-s","192.168.57.101:5555","shell","uiautomator","runtest","AppiumBootstrap.jar","-c","io.appium.android.bootstrap.Bootstrap","-e","pkg","com.xueqiu.android","-e","disableAndroidWatchers",false,"-e","acceptSslCerts",false]

# 33.查询WMS服务相关信息
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell dumpsys window'

# 34.启动待测app
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell am start -W -n com.xueqiu.android/.view.WelcomeActivityAlias -S -a android.intent.action.MAIN -c android.intent.category.LAUNCHER -f 0x10200000'

# 35. 新建AndroidDriver session
[Appium] New AndroidDriver session created successfully, session 76cf893d-f1f9-43d6-be5e-7f59ca25c3db added to master session list

# 36.设置AppiumDriver上下文
[HTTP] <-- POST /wd/hub/session 200 5331 ms - 959
[HTTP] --> POST /wd/hub/session/76cf893d-f1f9-43d6-be5e-7f59ca25c3db/context

[debug] [MJSONWP] Calling AppiumDriver.setContext() with args: ["NATIVE_APP","76cf893d-f1f9-43d6-be5e-7f59ca25c3db"]

# 37.获取设备上Unix域套接字信息
[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell cat /proc/net/unix'

# 获取页面源码
[HTTP] --> GET /wd/hub/session/76cf893d-f1f9-43d6-be5e-7f59ca25c3db/source

[debug] [MJSONWP] Calling AppiumDriver.getPageSource() with args: ["76cf893d-f1f9-43d6-be5e-7f59ca25c3db"]
[debug] [AndroidBootstrap] Sending command to android: {"cmd":"action","action":"source","params":{}}

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got data from client: {"cmd":"action","action":"source","params":{}}

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got command of type ACTION
[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got command action: source

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Returning result: {"status":0,"value":"<?xml version=\"1.0\" encoding=\"UTF-8\"?>页面源码</xml>"

# 截屏
[HTTP] --> GET /wd/hub/session/76cf893d-f1f9-43d6-be5e-7f59ca25c3db/screenshot

[debug] [MJSONWP] Calling AppiumDriver.getScreenshot() with args: ["76cf893d-f1f9-43d6-be5e-7f59ca25c3db"]
[debug] [ADB] Device API level: 23
[debug] [MJSONWP] Responding to client with driver.getScreenshot() result: "图片base64代码"

# 获取屏幕尺寸信息
[HTTP] --> GET /wd/hub/session/76cf893d-f1f9-43d6-be5e-7f59ca25c3db/window/current/size

[debug] [MJSONWP] Calling AppiumDriver.getWindowSize() with args: ["current","76cf893d-f1f9-43d6-be5e-7f59ca25c3db"]

[debug] [AndroidBootstrap] Sending command to android: {"cmd":"action","action":"getDeviceSize","params":{}}

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got data from client: {"cmd":"action","action":"getDeviceSize","params":{}}

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got command of type ACTION
[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got command action: getDeviceSize

[debug] [AndroidBootstrap] Received command result from bootstrap
[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Returning result: {"status":0,"value":{"height":2392,"width":1440}}
[debug] [MJSONWP] Responding to client with driver.getWindowSize() result: {"height":2392,"width":1440}

# 获取元素
[HTTP] --> POST /wd/hub/session/76cf893d-f1f9-43d6-be5e-7f59ca25c3db/elements
[HTTP] {"using":"id","value":"com.xueqiu.android:id/agree"}

[debug] [MJSONWP] Calling AppiumDriver.findElements() with args: ["id","com.xueqiu.android:id/agree","76cf893d-f1f9-43d6-be5e-7f59ca25c3db"]

[debug] [BaseDriver] Valid locator strategies for this request: xpath, id, class name, accessibility id, -android uiautomator

[debug] [BaseDriver] Waiting up to 0 ms for condition

[debug] [AndroidBootstrap] Sending command to android: {"cmd":"action","action":"find","params":{"strategy":"id","selector":"com.xueqiu.android:id/agree","context":"","multiple":true}}

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got data from client: {"cmd":"action","action":"find","params":{"strategy":"id","selector":"com.xueqiu.android:id/agree","context":"","multiple":true}}

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got command of type ACTION
[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got command action: find
[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Finding 'com.xueqiu.android:id/agree' using 'ID' with the contextId: '' multiple: true

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Using: UiSelector[RESOURCE_ID=com.xueqiu.android:id/agree]
[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] getElements selector:UiSelector[RESOURCE_ID=com.xueqiu.android:id/agree]

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Element[] is null: (0)

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] getElements tmp selector:UiSelector[INSTANCE=0, RESOURCE_ID=com.xueqiu.android:id/agree]

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Element[] is null: (1)

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] getElements tmp selector:UiSelector[INSTANCE=1, RESOURCE_ID=com.xueqiu.android:id/agree]

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Returning result: {"status":0,"value":[{"ELEMENT":"1"}]}

[debug] [AndroidBootstrap] Received command result from bootstrap
[debug] [MJSONWP] Responding to client with driver.findElements() result: [{"ELEMENT":"1"}]

# 点击元素
[HTTP] --> POST /wd/hub/session/76cf893d-f1f9-43d6-be5e-7f59ca25c3db/element/1/click

[debug] [MJSONWP] Calling AppiumDriver.click() with args: ["1","76cf893d-f1f9-43d6-be5e-7f59ca25c3db"]

[debug] [AndroidBootstrap] Sending command to android: {"cmd":"action","action":"element:click","params":{"elementId":"1"}}

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got data from client: {"cmd":"action","action":"element:click","params":{"elementId":"1"}}

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got command of type ACTION
[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got command action: click

[debug] [AndroidBootstrap] Received command result from bootstrap
[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Returning result: {"status":0,"value":true}
[debug] [MJSONWP] Responding to client with driver.click() result: true

# 发送数据 send_key 
[HTTP] --> POST /wd/hub/session/76cf893d-f1f9-43d6-be5e-7f59ca25c3db/element/3/value
[HTTP] {"value":["ali"]}

[debug] [MJSONWP] Calling AppiumDriver.setValue() with args: [["ali"],"3","76cf893d-f1f9-43d6-be5e-7f59ca25c3db"]

[debug] [AndroidBootstrap] Sending command to android: {"cmd":"action","action":"element:setText","params":{"elementId":"3","text":"ali","replace":false}}

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got data from client: {"cmd":"action","action":"element:setText","params":{"elementId":"3","text":"ali","replace":false}}

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got command of type ACTION
[AndroidBootstrap] [BOOTSTRAP LOG] [debug] Got command action: setText

[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Using element passed in: 3
[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Attempting to clear using UiObject.clearText().
[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Text remains after clearing, but it appears to be hint text.
[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Text not cleared. Assuming remainder is hint text.
[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Sending plain text to element: ali

[debug] [AndroidBootstrap] Received command result from bootstrap
[debug] [AndroidBootstrap] [BOOTSTRAP LOG] [debug] Returning result: {"status":0,"value":true}
[debug] [MJSONWP] Responding to client with driver.setValue() result: true

# Appium关闭
[Appium] Received SIGINT - shutting down

[Logcat] Logcat terminated with code null, signal SIGINT
[UiAutomator] UiAutomator exited unexpectedly with code null, signal SIGINT
[debug] [UiAutomator] Moving to state 'stopped'

[debug] [AndroidDriver] Shutting down Android driver

[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell am force-stop com.xueqiu.android'

[Appium] Closing session, cause was 'UiAUtomator shut down unexpectedly'
[Appium] Removing session 76cf893d-f1f9-43d6-be5e-7f59ca25c3db from our master session list

[debug] [ADB] Pressing the HOME button

[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell input keyevent 3'

[AndroidBootstrap] Cannot shut down Android bootstrap; it has already shut down

[debug] [Logcat] Stopping logcat capture
[debug] [Logcat] Logcat already stopped

[debug] [ADB] Running '~/Library/Android/sdk/platform-tools/adb -P 5037 -s 192.168.57.101\:5555 shell am force-stop io.appium.unlock'
```

## 参考

18.[mock_location](https://www.jianshu.com/p/baeed5eff82a)

29.[adb forward](http://www.cnblogs.com/wysk/archive/2017/12/21/7417122.html)

33.[adb shell dumpsys window](http://gityuan.com/2016/05/14/dumpsys-command/)

37.[adb shell cat /proc/net/unix