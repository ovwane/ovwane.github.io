---
title: Docker运行Redis
date: 2018-09-01 11:21:44
tags:
---

```shell
docker pull redis:4.0.11

docker run -p 6379:6379 -v ~/docker/redis/:/data  --name redis-4.0.11 -d redis:4.0.11
```

**客户端**

macOS 客户端 Redis Desktop Manager](http://docs.redisdesktop.com/en/0.9/)编译

```shell
#未测试
git clone --recursive https://github.com/uglide/RedisDesktopManager.git -b 0.9 rdm && cd ./rdm

Install XCode with Xcode build tools

Install Homebrew

#Copy 
cd ./src && cp ./resources/Info.plist.sample ./resources/Info.plist

# Building RDM dependencies require i.a. openssl and cmake. Install them: 
brew install openssl cmake

# Build RDM dependencies 
./configure

#Install Qt 5.9. Add Qt Creator and under Qt 5.9.x add Qt Charts module.
Open ./src/rdm.pro in Qt Creator

Run build
```

## 参考

[CentOS7+Docker+Redis配置](https://www.jianshu.com/p/fd64464e23d9)