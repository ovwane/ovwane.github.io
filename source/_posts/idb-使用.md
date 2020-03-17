---
title: idb 使用
date: 2020-03-04 17:19:48
tags:
---

# [idb](https://www.fbidb.io/)

> idb is a flexible command line interface for automating iOS simulators and devices



## 安装

1. 安装依赖 grpc

```
brew tap grpc/grpc && brew install grpc
```

2. 安装 idb-companion

```
brew tap facebook/fb && brew install idb-companion
```

3. 安装 idb-client

   > 要求 Python 3.6 +

```
pip install fb-idb
```



## 使用

启动一个 companion

```
idb_companion --udid F52D0B40-46D4-4B62-8AB5-8CADBF5C6E66
```



连接设备

```
idb connect 0B33F35A-3164-4C3D-9650-1672D0FE1B67
```

>Idb connect host port



查看设备信息

```
idb describe
```



查看设备列表

```
idb list-targets
```



启动设备

```
idb boot --udid F52D0B40-46D4-4B62-8AB5-8CADBF5C6E66
```



查看 app 列表

```
idb list-apps
```



安装 app

```
idb install test.ipa
```

>.app or .ipa



卸载 app

```
idb uninstall com.apple.Maps
```



启动 app

```
idb launch --udid F52D0B40-46D4-4B62-8AB5-8CADBF5C6E66 com.apple.Maps
```



关闭 app

```
idb terminate com.apple.Maps
```



截图

```
idb screenshot pic.png
```



录屏

```
idb record video --udid F52D0B40-46D4-4B62-8AB5-8CADBF5C6E66 video.mp4
```



查看日志

```
idb log --udid F52D0B40-46D4-4B62-8AB5-8CADBF5C6E66
```



查看测试列表

```
idb xctest list
```



## 参考

 [facebook/idb: idb is a flexible command line interface for automating iOS simulators and devices](https://github.com/facebook/idb) 

