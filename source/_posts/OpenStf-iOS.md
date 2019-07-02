---
title: OpenStf iOS
date: 2019-07-01 08:06:34
tags:
---

## OpenStf iOS 适配

- macOS 10.14.4
- node 8.15.0

- python 2.7.15
- Xcode 10.0

> nvm，pyenv，xcversion



### 1. 环境准备

  安装libimobiledevice等依赖工具，如果已经安装过，可能需要升级，先卸载，再安装最新版本

```shell
brew uninstall --ignore-dependencies usbmuxd
brew uninstall --ignore-dependencies libimobiledevice
brew uninstall --ignore-dependencies ideviceinstaller

brew install --HEAD usbmuxd
#brew unlink usbmuxd
#brew link usbmuxd

brew install --HEAD libimobiledevice
brew install --HEAD ideviceinstaller
brew install carthage
brew install socat
```



### 2. 安装 stf 依赖：

```shell
brew install graphicsmagick libsodium zeromq protobuf yasm pkg-config
```



### 3. 配置 WebDriverAgent

```shell
# git clone -b v0.1.1 https://github.com/mrx1203/WebDriverAgent.git
git clone https://github.com/mrx1203/WebDriverAgent.git

cd WebDriverAgent

# 修改 bundleID，修改证书，修改Schema
open WebDriverAgent.xcodeproj

# Validating Inspector 有问题，注释了。
./Scripts/bootstrap.sh
```

>使用的 Xcode 10.0，Xcode 10.2.1 版本 linker 有问题，提示找不到 XCEventGenerator。



### 4. 启动 ios-provider

- 拉取源代码

```shell
git clone https://github.com/mrx1203/stf.git
```

- 安装依赖库

```shell
cd stf
```

```shell
CXXFLAGS="-mmacosx-version-min=10.9" LDFLAGS="-mmacosx-version-min=10.9" npm install; npm install --save request; npm install --save request-promise
```

检查组件是否都正常

```shell
bin/stf doctor
```

- 启动ios-provider，假设主服务器的ip地址为10.8.8.128，该节点的ip地址为192.168.1.24。

```shell
bin/stf ios-provider --name "mac001" --connect-sub tcp://10.8.8.128:7250 --connect-push tcp://10.8.8.128:7270 --storage-url http://10.8.8.128 --public-ip 192.168.1.24 --heartbeat-interval 20000 --wda-path /Users/jinlong/projects/mrx1203/WebDriverAgent --wda-port 8100
```



挂载

```shell
ideviceimagemounter /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport/12.0/DeveloperDiskImage.dmg  /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport/12.0/DeveloperDiskImage.dmg.signature
```



截图

```shell
idevicescreenshot -u 81940f3e74cd76f934c7a6ebec5ea038eb3f7e1d 81940f3e74cd76f934c7a6ebec5ea038eb3f7e1d.png
```



## 改造官方 stf

### 拉取官方代码

> OpenStf 3.4.1

```shell
git clone -b v3.4.1 https://github.com/openstf/stf.git
```



### 复制代码到官方代码目录下

1. 拷贝`lib/cli`下的`ios-device,ios-provider,local`三个文件夹，local文件夹可以直接覆盖。
2. 拷贝`lib/units`下的`ios-device,ios-provider`。
3. 覆盖`lib/units/storage/plugins/apk/task/manifest.js`文件。
4. 拷贝`lib/util`下的`ipareader.js,download.js`文件；ipa文件解析和下载。
5. 覆盖`lib/wire/wire.proto`文件。
6. 覆盖`res/app/components/stf/install/install-service.js`文件；ipa文件的安装。
7. 覆盖`res/app/components/stf/storage/storage-service.js`文件；上传ipa文件。
8. 覆盖`package.json`添加了依赖模块。



安装依赖库

```shell
CXXFLAGS="-mmacosx-version-min=10.9" LDFLAGS="-mmacosx-version-min=10.9" npm install; npm install --save request; npm install --save request-promise
```



检查组件是否都正常

```shell
bin/stf doctor
```



复制 WebDriverAgent 到 stf 目录

```shell
cd stf
cp WebDriverAgent .
```



启动

```shell
bin/stf local
```



## 错误

[idevicescreenshot not working](https://reviewdb.io/questions/1522739809390/idevicescreenshot-not-working)

[xcode10版本的问题， 找不到XCElementSnapshot头文件](https://github.com/macacajs/XCTestWD/issues/141)

[node-gyp 'utility' file not found ](https://github.com/nodejs/node-gyp/issues/1564) `CXXFLAGS="-mmacosx-version-min=10.9" LDFLAGS="-mmacosx-version-min=10.9" npm i`

[iproxy 208 错误](https://github.com/appium/appium/issues/6996) `ps -ax|grep -i "iproxy"|grep -v grep|awk '{print "kill -9 " $1}'|sh`



## 参考

[STF 集成 iOS 之 开源了](https://testerhome.com/topics/19548)