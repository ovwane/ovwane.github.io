---
title: Python 配置
date: 2017-10-20 21:55:00
categories:
- 技术
- Python
tags:
- Python
- macOS
---

# Python 配置

> 10.12.6
>
> 10.13
>
> 10.13.4 2018-05-05 20:01
>
> 10.15.6



<!--more-->

## 安装

macOS ~~安装依赖~~

```shell
brew install readline openssl xz zlib
```



Ubuntu 安装依赖

```
apt install -y gcc make git patch zlib1g-dev libbz2-dev libsqlite3-dev python3-dev libxml2-dev libffi-dev libssl-dev libreadline6-dev libgdbm-dev libncurses5-dev

apt install -y curl wget 
```



CentOS 安装依赖

```
yum install -y  gcc gcc-c++ make git patch openssl-devel zlib-devel readline-devel sqlite sqlite-devel bzip2-devel curl wget ncurses-devel sqlite-devel gdbm-devel xz-devel tk-devel
```



### [pyenv](https://github.com/pyenv/pyenv)

安装pyenv

```shell
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```



添加环境变量

~/.zshrc

```shell
# pyenv start
export PYENV_ROOT="${HOME}/.pyenv"

if [ -d "${PYENV_ROOT}" ]; then
	export PATH="${PYENV_ROOT}/bin:${PATH}"
	eval "$(pyenv init -)"
fi
# pyenv end
```

重新加载shell

```shell
exec $SHELL -l
```



查看可安装Python版本

```
pyenv install -l
```



安装Python

[ERROR: The Python ssl extension was not compiled. Missing the OpenSSL lib?](https://github.com/pyenv/pyenv/wiki/Common-build-problems)

```shell
# 可以提前下载 python xz 文件，设置环境变量。
export PYTHON_BUILD_CACHE_PATH=~/Downloads

# 安装 Python 3.6.1，如果有问题可以添加 CFLAGS。
# CFLAGS="-I$(brew --prefix openssl)/include" \
# LDFLAGS="-L$(brew --prefix openssl)/lib" \
# CFLAGS="-I$(xcrun --show-sdk-path)/usr/include" \
pyenv install -v 3.6.1
```



设置默认版本

```shell
pyenv global 3.6.5
pyenv rehash
```



查看python版本

```
python -V
```



## 设置

### pip 使用豆瓣源

```
mkdir ~/.pip && cat >~/.pip/pip.conf<<EOF
[global]
index-url = https://pypi.douban.com/simple
--format=columns
EOF
```



更新pip

```shell
pip install --upgrade pip
```



### 安装插件

~~virtualenvwrapper~~

```shell
$ pip install virtualenvwrapper

# start virtualwrapper
PY=$HOME/.pyenv/versions/3.6.5/bin
VIRTUALENVWRAPPER_PYTHON=$PY/python
export VIRTUALENV_USE_DISTRIBUTE=1
export WORKON_HOME=~/.virtualenv
if [ -e $PY/virtualenvwrapper.sh ]; then
    source $PY/virtualenvwrapper.sh
else
    echo "error, cannot found virtualenvwrapper"
fi
export PIP_VIRTUALENV_BASE=$WORKON_HOME
export PIP_RESPECT_VIRTUALENV=true
export PIP_REQUIRE_VIRTUALENV=true
# end virtualwrapper
```



~~**pipenv**~~

```shell
brew install pipenv
```



~~弃用pip安装方式~~


```shell
pip install pipenv
```



~~pipenv环境变量~~

```shell
$ vim ~/.zshrc

# start pipenv
eval "$(pipenv --completion)"
# end pipenv
```



### python虚拟环境

```
# 新建虚拟环境
mkvirtualenv test
# 新建虚拟环境并指定python版本
mkvirtualenv django_xadmin --python=/Users/jinlong/.pyenv/versions/2.7.12/bin/python
# 使用虚拟环境
workon test
# 离开虚拟环境
deactivate
# 删除虚拟环境
rmvirtualenv test
```



## 参考

[Ubuntu 编译安装Python3.6所需依赖库及安装细节](https://www.jianshu.com/p/2fd6383ec010)






