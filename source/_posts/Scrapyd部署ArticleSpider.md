---
title: Scrapyd 部署 ArticleSpider
date: 2017-09-08 20:37:44
---

# Scrapyd 部署 ArticleSpider
#### 安装数据库
```
yum -y update
yum -y install epel-release
yum -y install mariadb-server mariadb mariadb-devel gcc redis
```

#### 启动
```bash
for services in mariadb redis; do
	systemctl enable $services
	systemctl start $services
	systemctl status $services
done	
```

#### mariadb 设置root密码
```
mysqladmin -u root password "root"

# 登陆测试
mysql -uroot -p

# or

mysql_secure_installation
# 登陆测试
mysql -uroot -p
```

#### 安装Python三方库
```
source /root/.pyenv/versions/3.6.2/envs/scrapyd/bin/activate

pip install scrapyd mmh3 elasticsearch-dsl Pillow PyMySQL pytesseract redis selenium mysqlclient
```

##### [/bin/sh mysql_config not found](http://blog.csdn.net/default7/article/details/73368665)
***
使用centos7安装python3，在安装 mysqlclient的时候报错 /bin/sh mysql_config not found 因为需要安装 mariadb-devel ，之后再报错error: command 'gcc' failed with exit status 1，缺乏 gcc。之后还是报错，因为 还是未安装 python36u-devel 
所以正确的安装应该是装完 yum install -y python36u 之后再安装 yum install python36u-devel mariadb-devel

yum install python36u python36u-devel
yum install gcc mariadb-devel
pip3 install mysqlclient
***

####
``` 
Project Name:
ArticleSpider

cd local ArticleSpider

scrapyd-deploy --build-egg output.egg

Upload EGG file:

Submit:

deploy success!
```