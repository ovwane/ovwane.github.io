

## 安装Python

### 安装Python依赖
```
# CentOS7
yum install -y  gcc gcc-c++ make git patch openssl-devel zlib-devel readline-devel sqlite sqlite-devel bzip2-devel curl wget ncurses-devel sqlite-devel gdbm-devel xz-devel tk-devel
```

### 安装[pyenv](https://github.com/pyenv/pyenv)
```
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

#### 配置bash_profile
```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc

echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc

exec "$SHELL"
```

#### 安装Python
```
pyenv install 3.6.5 -v

pyenv global 3.6.5

pyenv rehash
```

/root/.pyenv/versions/3.6.5/bin/python



```
pip install virtualenvwrapper pipenv
```

```shell

```

```shell
vim ~/.bashrc

#start virtualwrapper
VIRTUALENVWRAPPER_PYTHON=/root/.pyenv/versions/3.6.5/bin/python
export WORKON_HOME='~/.virtualenv'
source /root/.pyenv/versions/3.6.5/bin/virtualenvwrapper.sh
#end
```

pipenv --python=/root/.pyenv/versions/3.6.5/bin/python



因Supervisor现在不支持python3的版本需要安装Python 2.7.13

pyenv install 2.7.13 -v
pyenv global 2.7.13
pyenv rehash
### 更改pypi源
```
# 新建pip.conf存放目录
mkdir ~/.pip&&cd ~/.pip

cat >pip.conf<<EOF
[global]
index-url = https://pypi.douban.com/simple
EOF
```