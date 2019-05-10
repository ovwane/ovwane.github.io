---
title: Appium 源码分析-学习笔记
date: 2019-05-07 12:12:19
tags:

---



> 环境参考 appium 的 [travis.yml](https://github.com/appium/appium/blob/master/.travis.yml)
>
> 使用网络代理



### Node 

```
nvm use v10.15.0
```



### Python

```shell
pyenv shell 2.7.15
```



### 运行 Appium

```shell
cd ~/projects
git clone https://github.com/appium/appium.git
cd appium
npm install
npm run build
node .
```



### Hacking on Appium

```shell
npm install -g appium-doctor
appium-doctor --dev
```



### 安装依赖

```shell
idevicelocation
```



#### opencv4nodejs

```shell
pyenv shell 3.6.1
npm install -g opencv4nodejs
```



#### fbsimctl

```shell
# Get the Facebook Tap.
brew tap facebook/fb
# Install fbsimctl from master
brew install fbsimctl --HEAD
```

> [fatal: Unable to create '/Users/jinlong/Library/Caches/org.carthage.CarthageKit/dependencies/GCDWebServer/index.lock': Operation not permitted](https://github.com/facebook/idb/issues/432)
>
> ```
> brew uninstall --force carthage
> rm -rf ~/Library/Caches/org.carthage.CarthageKit
> ```



## [appium-packages](http://appium.io/docs/en/contributing-to-appium/appium-packages/)

[appium-packages 简述](https://github.com/appium/appium/blob/master/docs/cn/contributing-to-appium/appium-packages.md)

Selenium Packages

Core Packages

Utility Packages

Android Packages

iOS Packages

> packages 都在 node_modules 目录下。



#### 使用 IDE

Appium 使用 Visual Studio Code 开发的 https://github.com/appium/appium ，依据是 `.vscode` 目录。



#### 调试的配置文件

launch.json

```json
{
    // 使用 IntelliSense 了解相关属性。 
    // 悬停以查看现有属性的描述。
    // 欲了解更多信息，请访问: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [

        {
            "type": "node",
            "request": "launch",
            "name": "Launch Program",
            "program": "${workspaceFolder}/build/lib/main.js",//package.json中的默认入口
            "console" : "integratedTerminal"  //控制台信息的显示
        }
    ]
}
```



#### 源码

`lib` 子目录是 Appium 的源码目录。而里面的 `main.js` 是 Appium 的程序入口。

`lib/main.js`： 程序入口 `async function main`

`lib/appium.js`：方法 `createSession`,`executeCommand`

`node_modules/appium-base-driver/lib/protocol/route.js`： 方法`METHOD_MAP`

`node_modules/appium-base-driver/lib/protocol/protocol.js`：方法`routeConfiguringFunction`

`node_modules/appium-base-driver/lib/express/server.js`：方法 ` routeConfiguringFunction`

`node_modules/appium-support/lib/logging.js`：方法`getLogger`

`lib/logger.js`：给Appium打印的每行日志加上`[Appium]`的前缀



## 参考

[contributing-to-appium](https://github.com/appium/appium/tree/master/docs/cn/contributing-to-appium)

[appium 源码分析合集 (一)](https://testerhome.com/topics/2463)

[Appium 开发环境搭建合集](https://testerhome.com/wiki/developingappium)

[Appium 从入门到原理](https://bestswifter.com/appium/)

[Appium在vscode中调试说明](http://cming.site/2018/02/22/201802221759/)

[手机自动化测试：appium源码分析之bootstrap一](http://www.cnblogs.com/laoli0201/articles/4881492.html)

[Appium Android Bootstrap源码分析之简介](https://blog.csdn.net/zhubaitian/article/details/40619777)

[启动和日志分析](https://cstsinghua.github.io/2018/07/19/Appium启动和日志分析/)

