---
title: macOS 安装配置PyV8
date: 2018-07-27 11:19:31
tags:
---

# macOS 安装配置PyV8

> macOS 10.13.5

**安装依赖：** 
Boost

```shell
brew install boost 

brew install libboost-all-dev boost-python3 boost-build boost-mpi
```

安装v8

```shell
brew install v8
```

安装PyV8

```shell
pip install -e git://github.com/brokenseal/PyV8-OS-X#egg=pyv8
```

## 参考

[安装pyv8](https://blog.csdn.net/sc_lujun/article/details/69067543)

[mac下安装pyv8](http://rrifx.com/post/mac-install-pyv8/)

[PyV8-OS-X](https://github.com/brokenseal/PyV8-OS-X)

[【Python3】 PyV8的安装与使用](https://www.cnblogs.com/richerdyoung/p/8535149.html)

[pyv8的安装和使用：python中执行js代码](https://www.cnblogs.com/shengulong/p/8082768.html)