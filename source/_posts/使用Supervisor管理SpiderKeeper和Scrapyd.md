---
title: 使用Supervisor管理SpiderKeeper和Scrapyd
date: 2017-10-20 10:07:00
categories:
- 技术
- 运维
tags:
- CentOS
- Linux
- Supervisor
- SpiderKeeper
- Scrapy
- Scrapyd
- Python
---

使用[Supervisor](https://github.com/Supervisor/supervisor)管理[SpiderKeeper](https://github.com/DormyMo/SpiderKeeper)和[Scrapyd](https://github.com/scrapy/scrapyd)
CentOS 7.3.11
Python 3.6.2
Python 2.7.13

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

## 安装Supervisor [Supervisor使用简介](http://liuzxc.github.io/blog/supervisor/)
### 新建虚拟环境
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
files = /etc/supervisor/*.conf
```
```
[include]
files = /etc/supervisor/conf.d/*.conf
```

```
/root/.pyenv/versions/2.7.13/envs/supervisor/bin/supervisord -c /etc/supervisord.conf
```

```
vi /usr/lib/systemd/system/supervisord.service

[Unit]                                                              
Description=supervisord - Supervisor process control system for UNIX
Documentation=http://supervisord.org                                
After=network.target                                                

[Service]                                                           
Type=forking                                                        
ExecStart=/root/.pyenv/versions/2.7.13/envs/supervisor/bin/supervisord -c /etc/supervisord.conf             
ExecReload=/root/.pyenv/versions/2.7.13/envs/supervisor/bin/supervisorctl reload                            
ExecStop=/root/.pyenv/versions/2.7.13/envs/supervisor/bin/supervisorctl shutdown                            

[Install]                                                           
WantedBy=multi-user.target
```

### 启动supervisor
```
# 查看进程
ps aux | grep supervisord

systemctl daemon-reload
systemctl enable supervisord && systemctl start supervisord
systemctl status supervisord -l
systemctl stop supervisord
systemctl reload supervisord
```

## Scrapyd
### 新建虚拟环境
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
command=source /root/.pyenv/versions/3.6.2/envs/scrapyd/bin/activate
directory=/data/scrapyd
command=/root/.pyenv/versions/3.6.2/envs/scrapyd/bin/scrapyd
autostart=true
autorestart=true
redirect_stderr=true
EOF
```

### 重启supervisor
```
/root/.pyenv/versions/supervisor/bin/supervisorctl

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
command=/root/.pyenv/versions/3.6.2/envs/scrapyd/bin/spiderkeeper --port=80 --server=http://localhost:6800 --username=admin --password=admin
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

### Python路径和supervisor命令路径
```
/root/.pyenv/versions/3.6.2/envs/scrapyd/bin

/root/.pyenv/versions/supervisor/bin/supervisorctl

/root/.pyenv/versions/supervisor/bin/supervisord
```
