---
title: macOS 配置 Macaca
date: 2018-10-15 14:51:25
tags:
---

#### 安装node

## iOS



#### XCTestWD

```shell
git clone https://github.com/macacajs/XCTestWD.git

cd XCTestWD/XCTestWD
```

```shell
xcodebuild -project XCTestWD.xcodeproj \
           -scheme XCTestWDUITests \
           -destination 'platform=iOS Simulator,name=iPhone 6' \
           XCTESTWD_PORT=8001 \
           clean test
```



#### Xcode



### Driver Installation

```
npm i macaca-ios -g
```



[debug log](https://github.com/macacajs/XCTestWD/blob/master/README.md#43-debug-info) will be displayed when ‘–verbose’ is set as an argument when initiating macaca.

- 查看TEAM_ID

```shell
DEVELOPMENT_TEAM_ID=TEAM_ID npm i macaca-ios -g
```



## Android

- [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (Java 9 is not supported)
- Android SDK
- Set the `ANDROID_HOME` environment variable to your `~/.bashrc`, `~/.bash_profile`, `~/.zshrc` or 

[gradle](https://gradle.org/) is needed in order to build [UIAutomatorWD](https://github.com/macacajs/UIAutomatorWD) and other package

```shell
brew install gradle
```

- If you got a error like [You have not accepted the license agreements of the following SDK components] on your install command [npm i macaca-android -g]，plz accept all Android SDK licenses uses command below, and retry install.

```shell
yes | $ANDROID_HOME/tools/bin/sdkmanager --licenses
```

#### ChromeDriver



## Macaca Cli

```shell
npm i -g macaca-cli
```

#### Environment

Let’s check the version and verify the environment.

```shell
# electron
#npm install electron -g
npm install macaca-electron -g

# show version
macaca -v

# verify environment
macaca doctor
```

#### 启动Macaca

```shell
macaca server --verbose
```



## 获取设备 ID

### iOS

#### **命令行方式**

```shell
xcrun simctl list
```

> 这行命令会列出你的所以模拟器信息，里面有类似 XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 的代码，就是模拟器 UDID。



#### **从 Xcode 获取**

打开模拟器，从菜单中打开 Hardware - devices - manage devices。 然后你会看到模拟器信息界面，里面有个 identifier，就是 UDID。



### Android

#### 命令行

> 先启动你的设备，然后使用 adb 命令查看设备信息：

```shell
adb devices
```



## [Macaca App Inspector](http://macacajs.github.io/app-inspector)

### 安装 Node

```shell
nvm install v8.16.0
```



### 安装 app-inspector

```shell
npm install app-inspector -g
```

> 模拟器就可以使用了。



### 启动 app-inspector

```shell
app-inspector -u YOUR-DEVICE-ID
```



### 打开 app-inspector 界面

你的命令行将输出如下的文字：

```shell
inspector start at: http://192.168.10.100:5678
```

浏览器里面打开输出的链接：http://127.0.0.1:5678。

> 推荐用 Chrome 浏览器。



### iOS 真机调试

#### 安装 [usbmuxd](https://github.com/libimobiledevice/usbmuxd) 以便于通过 USB 通道测试 iOS 真机

```bash
brew install usbmuxd
```



#### 安装

```shell
brew install libimobiledevice
```



#### 安装 [ideviceinstaller](https://github.com/libimobiledevice/ideviceinstaller) 用来给真机安装 App

```bash
brew install ideviceinstaller
```



#### 应用中如含有 WebView，请安装 [ios-webkit-debug-proxy](https://github.com/google/ios-webkit-debug-proxy)

```bash
brew install ios-webkit-debug-proxy
```



#### 设置环境变量

`vim ~/.zshrc`

```
TEAM_ID=C8V426QHXX
```

> TEAM_ID 是开发者团队 id，十位数的字符串。



#### 安装 app-inspector

```shell
DEVELOPMENT_TEAM_ID=TEAM_ID npm i app-inspector -g
```



#### 使用 Xcode 修改 XCUITest 的证书

```shell
cd ~/.nvm/versions/node/v8.16.0/lib/node_modules/app-inspector/node_modules/xctestwd/XCTestWD
```

> 修改 Schema 为：



#### 安装 iOS driver

```shell
DEVELOPMENT_TEAM_ID=TEAM_ID npm i macaca-ios -g
```



## Docker 运行Macaca

[macacajs/macaca-android-docker](https://hub.docker.com/r/macacajs/macaca-android-docker/tags)

```shell
docker pull macacajs/macaca-android-docker:latest
```

[macacajs/macaca-datahub](https://hub.docker.com/r/macacajs/macaca-datahub)

```shell
docker pull macacajs/macaca-datahub:latest
```

```shell
docker run -d -p 9200:9200 -p 9300:9300 macacajs/macaca-datahub:latest
```



## 参考

[Macaca Environment Setup](https://macacajs.github.io/environment-setup)

[Macaca • 面向多端的自动化测试](https://macacajs.github.io/zh/run-with-docker#%E4%BD%BF%E7%94%A8-docker)