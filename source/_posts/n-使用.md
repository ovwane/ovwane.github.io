---
title: n 使用
date: 2021-02-04 16:57:01
tags:
---

使用 n 来管理 node 多版本

<!--more-->

安装 n

```shell
brew install n
```



设置环境变量 ~/.zshrc

```shell
export N_PREFIX=$HOME/.n
export PATH=$N_PREFIX/bin:$PATH
export N_USE_XZ=1
```



安装 node

```shell
n 10.13.0
```