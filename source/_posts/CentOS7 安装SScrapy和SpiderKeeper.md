---
date: 2017-05-08 13:23:49
---

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
vim ~/.bashrc

#start virtualwrapper
VIRTUALENVWRAPPER_PYTHON=/root/.pyenv/versions/3.6.5/bin/python
export WORKON_HOME='~/.virtualenv'
source /root/.pyenv/versions/3.6.5/bin/virtualenvwrapper.sh
#end
```

pipenv --python=/root/.pyenv/versions/3.6.5/bin/python



因Supervisor现在不支持python3的版本需要安装Python 2.7.14

pyenv install 2.7.14 -v
pyenv global 2.7.14
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
## 安装Supervisor [Supervisor使用简介](http://liuzxc.github.io/blog/supervisor/)

### 新建虚拟环境

```shell
cd /usr/local/
mkdir py2 py3
```

```shell
cd /usr/local/py2
pipenv --python=/root/.pyenv/versions/2.7.14/bin/python

workon py2
```



```
pyenv virtualenv 2.7.13 supervisor
```

### 激活虚拟环境

```
source /root/.pyenv/versions/2.7.13/envs/supervisor/bin/activate
```

### 安装supervisor

```
pip install supervisor
```

```
mkdir -p /etc/supervisor/

echo_supervisord_conf > /etc/supervisord.conf
```

```
echo_supervisord_conf > /etc/supervisor/supervisord.conf
```

```
vim /etc/supervisord.conf

[include]
files = /etc/supervisor/conf.d/*.conf
```

```
/root/.virtualenv/py2-kYuLH55n/bin/supervisord -c /etc/supervisord.conf
```

```
vim /usr/lib/systemd/system/supervisord.service

[Unit]                                                              
Description=supervisord - Supervisor process control system for UNIX
Documentation=http://supervisord.org                                
After=network.target                                                

[Service]                                                           
Type=forking                                                        
ExecStart=/root/.virtualenv/py2-kYuLH55n/bin/supervisord -c /etc/supervisord.conf             
ExecReload=/root/.virtualenv/py2-kYuLH55n/bin/supervisorctl reload                            
ExecStop=/root/.virtualenv/py2-kYuLH55n/bin/supervisorctl shutdown                            

[Install]                                                           
WantedBy=multi-user.target
```

### 启动supervisor

```
# 查看进程
ps aux | grep supervisord

systemctl daemon-reload
systemctl enable supervisord
systemctl start supervisord
systemctl status supervisord -l
systemctl stop supervisord
systemctl reload supervisord
```

## Scrapyd

### 新建虚拟环境

```shell
cd /usr/local/py3
pipenv --python=/root/.pyenv/versions/3.6.5/bin/python

workon py3-hv-Nhry6
```



```
pyenv virtualenv 3.6.2 scrapyd

source /root/.pyenv/versions/3.6.2/envs/scrapyd/bin/activate

pyenv activate scrapyd

pyenv deactivate
```

### 安装scrapyd

```
pip install scrapyd
```

mkdir -p /data/scrapyd

### 配置scrapyd

/etc/supervisor/scrapyd.conf

```
cat >/etc/supervisor/scrapyd.conf<<EOF
[program:scrapyd]
command=workon py3-hv-Nhry6
directory=/data/scrapyd
command=/root/.virtualenv/py3-hv-Nhry6/bin/scrapyd
autostart=true
autorestart=true
redirect_stderr=true
EOF
```

### 重启supervisor

```
/root/.virtualenv/py2-kYuLH55n/bin/supervisorctl

status

reread|reload
```

## 安装SipderKeeper

### 安装

```
source /root/.pyenv/versions/3.6.2/envs/scrapyd/bin/activate

pip install spiderkeeper
```

mkdir -p /data/spiderkeeper

### 配置spiderkeeper

/etc/supervisor/spiderkeeper.conf

```
cat >/etc/supervisor/spiderkeeper.conf<<EOF
[program:spiderkeeper]
command=source /root/.pyenv/versions/3.6.2/envs/scrapyd/bin/activate
directory=/data/spiderkeeper
command=/root/.virtualenv/py3-hv-Nhry6/bin/spiderkeeper --port=8000 --server=http://localhost:6800 --username=admin --password=admin
autostart=true
autorestart=true
startretries=3
EOF
```

### 重启supervisor

```
/root/.pyenv/versions/supervisor/bin/supervisorctl

status

reread|reload
```

### 