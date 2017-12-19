[distribute_crawler项目实战](http://blog.csdn.net/u011008734/article/details/47259197)
[distribute_crawler](https://github.com/aware-why/distribute_crawler)

Python编译依赖

```
apt-get install -y zlib1g-dev libssl-dev libbz2-dev libreadline-dev libsqlite3-dev
```

配置 pyenv

```
git clone https://github.com/pyenv/pyenv.git ~/.pyenv

echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile

echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile

echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bash_profile
```
```
pyenv install 3.6.3
pyenv global 3.6.3
```

更改pypi源

```
# 新建pip.conf存放目录
mkdir ~/.pip&&cd ~/.pip

cat >pip.conf<<EOF
[global]
index-url = https://pypi.douban.com/simple
EOF
```


```
pip install pipenv
```

```
cd project
pipenv install 
pip install scrapy redis pymongo
```

```
apt-get install -y redis-server
```

```
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6

echo "deb [ arch=amd64,arm64 ] http://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list

apt-get update

apt-get install -y mongodb-org

systemctl enable mongod

systemctl start mongod
```

```
apt-get install -y apache2 libapache2-mod-wsgi \
python-twisted python-cairo python-django-tagging

pip install Django==1.5.2 tagging carbon whisper graphite-web parse_lookup
```