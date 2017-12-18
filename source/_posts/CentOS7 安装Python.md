## 安装Python
### 安装Python依赖
```
# CentOS7
yum install -y  gcc gcc-c++ make git patch openssl-devel zlib-devel readline-devel sqlite sqlite-devel bzip2-devel curl wget ncurses-devel sqlite-devel gdbm-devel xz-devel tk-devel
```

### 安装[pyenv](https://github.com/pyenv/pyenv)
```
curl -L https://raw.githubusercontent.com/yyuu/pyenv-installer/master/bin/pyenv-installer | bash
```

#### 配置bash_profile
```
# 添加参数
vim ~/.bash_profile

export PATH="/root/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"

# 立即生效
source ~/.bash_profile'
```

#### 设置pyenv下载源为本地目录
```
wget https://www.python.org/ftp/python/3.6.2/Python-3.6.2.tar.xz

# 新建存放Python-3.6.2.tar.xz的目录
mkdir /root/python/ && cd /root/python/

# 设置变量
export PYTHON_BUILD_CACHE_PATH=/root/python

# 设置变量
export PYTHON_BUILD_MIRROR_URL=/root/python

# 查看变量设置
env |grep PYTHON_BUILD_MIRROR_URL
```

### 安装Python
```
pyenv install 3.6.2 -v

pyenv global 3.6.2

pyenv rehash
```

***
因Supervisor现在不支持python3的版本需要安装Python 2.7.13

pyenv install 2.7.13 -v</br>
pyenv global 2.7.13</br>
pyenv rehash
***

### 更改pypi源
```
# 新建pip.conf存放目录
mkdir ~/.pip&&cd ~/.pip

cat >pip.conf<<EOF
[global]
index-url = https://pypi.douban.com/simple
EOF
```