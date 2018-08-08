---
title: Ubuntu 安装配置PyV8
date: 2018-07-30 11:24:21
tags:
---

# Ubuntu 安装配置PyV8

> Ubuntu 18.04.1 LTS

**安装依赖：** 
Boost

```shell
apt install -y scons
#libboost-all-dev
apt install -y libboost-dev libboost-thread-dev libboost-system-dev libboost-python-dev
```

安装v8

```shell
brew install v8
```

安装PyV8

```shell
pip install pyv8
```

## 参考

[Ubuntu下安装boost库](https://blog.csdn.net/zhangxiao93/article/details/51077933)

[记录Ubuntu & Windows下安装PyV8](http://shomy.top/2016/03/11/ubuntu-python-pyv8/)