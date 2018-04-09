title: macOS 配置Python环境
date: 2017-10-20 21:55:00
categories:
- 技术
- Python
tags:
- Python
- macOS
---
macOS 配置Python环境
10.12.6
10.13

### 安装pyenv
```
brew install pyenv
```

### 安装Python
[ERROR: The Python ssl extension was not compiled. Missing the OpenSSL lib?](https://github.com/pyenv/pyenv/wiki/Common-build-problems)

```
# 设置变量
export PYTHON_BUILD_CACHE_PATH=/root/python
# 安装 Python 3.6.3
CFLAGS="-I$(brew --prefix openssl)/include" \
LDFLAGS="-L$(brew --prefix openssl)/lib" \
pyenv install -v 3.6.3
```

```
pyenv install 3.6.3
pyenv install 2.7.14

# 添加环境变量
vim ~/.zshrc
#start pyenv
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
#end

# 重新加载shell
exec $SHELL -l

# 设置 3.6.3 为默认版本
pyenv global 3.6.3
pyenv rehash

# 查看python版本
python -V
```

### 设置pip 使用豆瓣源
```
mkdir ~/.pip

cat >~/.pip/pip.conf<<EOF
[global]
index-url = https://pypi.douban.com/simple
--format=columns
EOF
```

### 安装Python插件
```
pip install virtualenvwrapper

# 添加环境变量
vim ~/.zshrc
#start virtualwrapper
VIRTUALENVWRAPPER_PYTHON=/Users/jinlong/.pyenv/versions/3.6.3/bin/python
export WORKON_HOME='~/.virtualenv'
source /Users/jinlong/.pyenv/versions/3.6.3/bin/virtualenvwrapper.sh
#end
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






