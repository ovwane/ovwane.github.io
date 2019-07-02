---
title: RVM 配置
date: 2019-06-11 11:52:36
tags:
---

# [RVM](https://github.com/rvm/rvm)

安装

```shell
curl -sSL https://get.rvm.io | bash -s stable
```



查看版本

```shell
rvm list known
```



安装 ruby

```shell
rvm install 2.6.3
```



设置默认 ruby

```shell
rvm alias create default 2.6.3
```