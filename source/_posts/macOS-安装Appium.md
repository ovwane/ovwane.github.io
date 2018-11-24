---
title: macOS 安装Appium
date: 2018-11-24 10:56:00
tags:
---

**依赖**

Java 1.8.0_181

Python 2.7.15

Node v8.12.0

Chromedriver 2.42

Selendroid standalone server 0.17.0



**安装**

```shell
pyenv shell 2.7.15

nvm install v8.12.0

nvm use v8.12.0

npm install -g appium
```



**安装日志**

```verilog
$ npm install -g appium
/Users/jinlong/.nvm/versions/node/v8.12.0/bin/appium -> /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/build/lib/main.js

> appium-chromedriver@4.5.0 install /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/appium-chromedriver
> node install-npm.js

[11:37:33] [Chromedriver Install] Installing Chromedriver version '2.42' for platform 'mac' and architecture '64'
[11:37:33] [Chromedriver Install] Opening temp file to write 'chromedriver_mac64' to...
[11:37:33] [Chromedriver Install] Opened temp file '/var/folders/dr/38ggmd7n691cx154cwz2xpqc0000gn/T/20181024-72948-9y4yab.11wh/chromedriver_mac64.zip'
[11:37:33] [Chromedriver Install] Downloading https://chromedriver.storage.googleapis.com/2.42/chromedriver_mac64.zip...
[11:38:46] [Chromedriver Install] Writing binary content to /var/folders/dr/38ggmd7n691cx154cwz2xpqc0000gn/T/20181024-72948-9y4yab.11wh/chromedriver_mac64.zip...
[11:38:46] [Chromedriver Install] Extracting /var/folders/dr/38ggmd7n691cx154cwz2xpqc0000gn/T/20181024-72948-9y4yab.11wh/chromedriver_mac64.zip to /var/folders/dr/38ggmd7n691cx154cwz2xpqc0000gn/T/20181024-72948-9y4yab.11wh/chromedriver_mac64
[11:38:47] [Chromedriver Install] Creating /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/appium-chromedriver/chromedriver/mac...
[11:38:47] [Chromedriver Install] Copying unzipped binary, reading from /var/folders/dr/38ggmd7n691cx154cwz2xpqc0000gn/T/20181024-72948-9y4yab.11wh/chromedriver_mac64/chromedriver...
[11:38:47] [Chromedriver Install] Writing to /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/appium-chromedriver/chromedriver/mac/chromedriver...
[11:38:47] [Chromedriver Install] /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/appium-chromedriver/chromedriver/mac/chromedriver successfully put in place

> appium-selendroid-driver@1.12.0 install /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/appium-selendroid-driver
> node ./bin/install.js

[11:38:48] Java version 1.8.0_181 found
[11:38:48] Ensuring /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/appium-selendroid-driver/selendroid/download exists
[11:38:48] Downloading Selendroid standalone server version 0.17.0 from http://repo1.maven.org/maven2/io/selendroid/selendroid-standalone/0.17.0/selendroid-standalone-0.17.0-with-dependencies.jar --> /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/appium-selendroid-driver/selendroid/download/selendroid-server-7cf7163ac47f1c46eff95b62f78b58c1dabdec534acc6632da3784739f6e9d82.jar
[11:41:51] Writing binary content to /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/appium-selendroid-driver/selendroid/download/selendroid-server.jar.tmp
[11:41:51] Selendroid standalone server downloaded
[11:41:51] Determining AndroidManifest location
[11:41:51] Determining server apk location
[11:41:52] Extracting manifest and apk to /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/appium-selendroid-driver/selendroid/download
[11:41:52] Copying manifest and apk to /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/appium-selendroid-driver/selendroid
[11:41:52] Cleaning up temp files
[11:41:52] Fixing AndroidManifest icon bug

> appium-windows-driver@1.4.0 install /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/appium-windows-driver
> node install-npm.js

Not installing WinAppDriver since did not detect a Windows system

> fsevents@1.2.4 install /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/fsevents
> node install

[fsevents] Success: "/Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/fsevents/lib/binding/Release/node-v57-darwin-x64/fse.node" already installed
Pass --update-binary to reinstall or --build-from-source to recompile

> heapdump@0.3.9 install /Users/jinlong/.nvm/versions/node/v8.12.0/lib/node_modules/appium/node_modules/heapdump
> node-gyp rebuild

  CXX(target) Release/obj.target/addon/src/heapdump.o
  SOLINK_MODULE(target) Release/addon.node
+ appium@1.9.1
added 592 packages from 434 contributors in 272.662s
```

