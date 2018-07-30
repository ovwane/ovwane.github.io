---
title: Ubuntu 安装 Python
date: 2018-07-30 09:37
---

# 安装Python

> Ubuntu 18.04.1 LTS

### 安装Python依赖

```
apt install -y gcc make git patch zlib1g-dev libbz2-dev libsqlite3-dev python3-dev libxml2-dev libffi-dev libssl-dev libreadline6-dev libgdbm-dev libncurses5-dev

apt install -y curl wget 
```

### 安装[pyenv](https://github.com/pyenv/pyenv)

```shell
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

#### 配置bash_profile

```shell
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc

echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc

exec "$SHELL"
```

#### 安装Python

```shell
# 离线安装Python，Python-3.6.1.tar.xz存放的位置
export PYTHON_BUILD_CACHE_PATH=~/Downloads

pyenv install 3.6.1 -v

pyenv global 3.6.1

pyenv rehash

#更新pip
pip install --upgrade pip
```

/root/.pyenv/versions/3.6.1/bin/python

```
pip install virtualenvwrapper pipenv
```

```shell
vim ~/.bashrc

#start virtualwrapper
VIRTUALENVWRAPPER_PYTHON=/root/.pyenv/versions/3.6.5/bin/python
export WORKON_HOME='~/.virtualenv'
source /root/.pyenv/versions/3.6.5/bin/virtualenvwrapper.sh
#end
```

### 更改pypi源

```
# 新建pip.conf存放目录
mkdir ~/.pip&&cd ~/.pip

cat >pip.conf<<EOF
[global]
index-url = https://pypi.douban.com/simple
EOF
```
## 参考

[Ubuntu 编译安装Python3.6所需依赖库及安装细节](https://www.jianshu.com/p/2fd6383ec010)